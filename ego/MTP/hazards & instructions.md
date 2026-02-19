#Hazards

in a pipelined system, multiple instructions are handled concurrently.
when one instruction is *dependent* on the results of another that has not yet completed, a **hazard** occurs.

when one instruction writes to a register and subsequent instructions read this register, *read after write* hazard. 

software interlocks like inserting NOP instructions (either programmer or compiler) complicate programming and degrades performance.
`MOV R0,R0`
all pipeline stages still run in NOP instr, but the control signals are arranged so that no architectural state changes occur.

hazards are classified as:
- *data hazard*, occurs when an instr reads register not yet written
- *control hazard*, when decision of what instruction to fetch next has not been made by the time fetch takes place.


some data hazards can be solved by *forwarding* a result from memory or write back stage to a dependent instruction in execute stage.
![[forwarding.png]]

required mux in front of ALU to select data from either register file or memory or write back stage.

forwarding is necessary when an instruction in the execute stage has source register matching with destination register of an instruction in memory or writeback stage.

add a hazard unit and two forwarding multiplexers.
the hazard unit receives four match signals from the datapath that indicate whether the source registers in the execute stage match the destination registers in the memory and write back stages.

the hazard unit also receives RegWrite signals from memory and write back stages to know whether the destination register will actually be written. eg STR,B doesnt write results to register file.
![[hazard_unit.png]]
![[hazard_match.png|270]]
these signals are abbreviated to `Match` above.

RA1E and RA2E are pipeline registers that stores register addresses from decode stage.

if both memory and writeback stages contain matching destination registers, then the memory stage should have priority, because it contains the more recently executed instruction.
![[forwarding_match_logic.png]]

forwarding is sufficient to solve RAW data hazards when the result is computed in the execute stage and then be forwarded to execute stage of next instr.

LDR does not finish reading data until the end of memory stage.
LDR has two-cycle latency (ALU+M).
this can't be fixed by forwarding.

alternative solution is to *stall* the pipeline, holding up operation until the data is available.
unused stage propagating through the pipeline is called a *bubble*. it behaves like a NOP instruction.
the bubble is introduces by zeroing out the execute stage control signals during a decode stall so that the bubble performs no actions and changes no architectural state.

Decode does not “fail.” The control unit always successfully decodes the instruction bits. A stall is not a decode failure—it is a hazard detected by the Hazard Unit (e.g., load-use hazard). When hazard logic detects that the instruction in Decode cannot safely proceed to Execute (because needed data isn’t ready), it asserts StallD and StallF and FlushE. What happens then: PC and IF/ID register are frozen → Fetch does not move to next instruction; the instruction currently in Decode stays there; the ID/EX pipeline register is cleared → a bubble (NOP) is inserted into Execute. So yes, a NOP appears in Execute when Decode is stalled. Decode does not “redo work”; it simply holds the same instruction. The next instruction is not fetched again because Fetch is stalled (PC not incremented). Everything is controlled by hazard detection logic.

When a NOP (bubble) is inserted into Execute, it naturally flows forward to Memory and then Writeback in the following cycles. You don’t need to separately insert NOPs into M and WB. You only clear (flush) the ID/EX register to create one bubble in E; after that, pipeline movement automatically carries that bubble through M and WB. The key idea: one inserted bubble occupies one pipeline slot and propagates forward by itself.

in summary, stalling a stage is performed by disabling the pipeline registers so that the contents do not change. when a stage is stalled, all previous stages must also be stalled, so that no subsequent instructions are lost.
the pipeline register directly after the stalled stage must be flushed to prevent bogus information from propagating forward.
stalls degrade performance.
![[stall_ldr.png]]

the hazard unit examines the instruction in the execute stage. if it is an LDR and its destination register (WA3E) matches either source operand of the instruction in decode stage (RA1D or RA2D), then that instruction must be stalled in the decode stage until the source operand in ready.

stalls are supported by adding enable inputs (EN) to the Fetch and Decode pipeline registers and a synchronous reset/clear (CLR) input to the execute pipeline registers.
when an LDR stall occurs, StallD and StallF are asserted to force the Decode and Fetch stage pipeline registers to hold their old values.
FlushE is also asserted to clear the contents of the execute stage pipeline registers, introducing a bubble.
![[cond_LDR.png|400]]
![[LDR_hazard_unit.png]]

#### Instructions
*logical instructions*
![[logical_instr.png|500]]
*shift instructions*
![[shift_instructions.png|500]]

in ARM multiplication of two 32-bit numbers produces 32/64 bit results.

`MUL R1,R2,R3` - multiplies R2 * R3 and places least 32bits in R1, discards most 32bits. used for small numbers
`UMULL` - unsinged multiply long, 64bit result
`SMULL` - singed long multiply

multiply accumulate, add product to a running sum.
`MLA UMLAL, SMLAL`


ARM instructions optionally set *condition flags*, subsequent instructions then execute *conditionally*, depending on the state of those condition flags.
![[condition_flags.png|500]]

these flags are set by ALU and are held in top 4 bits of the 32-bit *current program status register (CPSR)*

#branching

ARM uses *branch instructions* to skip over sections of code or repeat code.
branch instructions change the program counter.

ARM includes two type of branches:
- *branch(B)*
- *branch and link(BL)*

*unconditional branch*
![[branch_no_condition.png|340]]
*conditional branch*
![[branch_conditional.png|500]]

Assembly code uses *labels* to indicate instruction locations
when assembly code is translated to machine code, these labels are translated into instruction addresses.
labels must not be indented, instr must be with one space in ARM.
![[conditional_mnemonics.png|500]]
![[if_branch.png]]

any instruction can be conditionally executed
```pug
CMP R0,R1
ADDEQ R2,R3,#1   // exec if Z==1
SUB R2,R2,R3
```

branches sometime introduce extra delay, whereas conditional execution is always fast.
when a block of code has a single instruction, it is better to
use conditional execution rather than branch around it. As the block
becomes longer, the branch becomes valuable because it avoids wasting
time fetching instructions that will not be executed.

assembly code tests the opposite condition of the one in the high-level code.
![[cond_instr_if_else.png|500]]
![[switch_asm.png|600]]
![[while_asm.png]]


![[branch_instructiosn.png|600]]

the processor calculates the *branch target address* from the instruction by sign-extending the 24-bit immediate, shifting it left by 2 (to convert words to bytes) and adding it to PC+8.
###### solving control hazards
the `B` instruction presents a control hazard: the pipelines processor does not know what instruction to fetch next, because the branch decision has not been made by the time next instruction is fetched. Writes to R15(PC) present a similar control hazard.

one mechanism for dealing with the control hazard is to stall the pipeline until the branch decision is made (PCsrcW is computed).
decision is made in the WB stage, pipelines have to be stalled for 4 cycles at every branch, this degrades performance. 

in the pipeline presented so far, the processor predicts that branches are not taken and simply continues executing program in order until PCSrcW is asserted to select next PC from ResultW. if the branch should have been taken, four instructions following the branch must be *flushed* by clearing the pipeline registers for those instructions. these wasted instruction cycles are called *branch misprediction penalty*
![[branch_flush.png]]

branch decision can be made in execute stage when the destination address has been computed and `CondEx` is known.
now the branch misprediction penalty has been reduced to two cycles.

a branch multiplexer has been added before the PC register to select the branch destination from ALUResultE. 
*BranchTakenE* signal controlling this multiplexer is asserted on branches whose condition is satisfied.
PCSrcW is now only asserted for writes to PC, which still occur in writeback stage.
![[earlier_branch_prediction.png]]
![[pipelined_full_hazard.png]]

*branch predictor*
![[two-bit_branch_predictor.png]]
A two-bit dynamic branch predictor solves this problem by having
four states: strongly taken, weakly taken, weakly not taken, and strongly not taken, as shown in Figure 7.62. When the loop is repeating, it enters the “strongly not taken” state and predicts that the branch should not be taken next time. This is correct until the last branch of the loop, which is taken and moves the predictor to the “weakly not taken” state. When the loop is first run again, the branch predictor correctly predicts that the branch should not be taken and re-enters the “strongly not taken” state.In summary, a two-bit branch predictor mispredicts only the last branch of a loop.
it operates in fetch stage.


At the rising edge:  
• EX→MEM register latches the ALU result (address) and control signals.  
• That address immediately appears at the memory input (after small register delay).

During the SAME cycle (the MEM stage cycle):  
• Memory sees a stable address.  
• Because it is synchronous, it internally registers the address at that clock edge.  
• It then uses the rest of the cycle to access memory.  
• By the end of the cycle, read data is valid at memory output.

At the NEXT rising edge:  
• MEM→WB pipeline register captures that read data.

So memory does NOT wait for an extra cycle beyond the MEM stage.  
The access happens between two clock edges:  
Edge 1 → address registered  
Edge 2 → data captured into MEM→WB.