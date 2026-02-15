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

