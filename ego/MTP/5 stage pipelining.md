#### MicroArchitecture
microarchitecture is the specific arrangement of registers, ALU's, memories, finite state machines and other logic building blocks needed to implement an architecture.

architectural state refers to the minimum set of elements that define the processor's behavior at any instant.

the *architectural state* for the ARM processor consists of 16 32-bit registers and the status register.
if you stop the processor and look at these values, you fully know what the processor is doing.
- R0-R12    general purpose        data,addresses,arguments
- R13       SP(stack pointer)      top of stack
- R14       LR(link register)      return address
- R15       PC(program counter)    Address of next instruction

- status register  CPSR (current program status register)

*register*
an n-bit register is a bank of n flip-flops that share a common CLK input, so that all bits of the register are updated at the same time.

the output of *combinational logic* depends only on the current input values.
the output of *sequential logic* depends on both current and prior input values.

bistable element, two stable states.
metastable, third possible state between 0 and 1.

an element with N stable states conveys $log_{2}{N}$ bits of information.

when the power is first applied to a sequential circuit, the initial state is unknown and unpredictable. eg. cross coupled inverters
![[cross_couples_not.png|250]]
user control is needed.
latches and flip-flops provide inputs to control the value of the *state variable*.
###### SR Latch
![[SR_LATCH.png|250]]
cross coupled NOR
![[SR_table.png|250]]
after applying forbidden control inputs Q=!Q=0, but if i now apply retain then $Q$ and $\overline{Q}$ can be either $0\ 1$ or $1\ 0$
###### D Latch
it has two inputs, the data input $D$ controls what the next state should be. the clock input, $CLK$ controls when the state should change.
![[D_LATCH.png]]

clock controls when data flows through the latch

latches are level triggered, so when clock is high, if $D$ changes multiple times then $Q$ also changes with it.
flip-flops are edge triggered.
![[D_FLIP_FLOP.png|200]]
NAND,NOR gates uses 4 transistors, NOT uses 2 transistors
AND is made up of 1 NAND and 1 NOT gate uses 6 transistors
![[4bit_register.png|240]]

*bus* - a bus is a set of parallel signal lines that carries data,address,control information between components

*positive edge producer*
![[positive_edge_detector.png|500]]

RC and not-and pulse generator are unreliable. we use master-slave.
###### Instruction set
computer instructions indicate both the operation to perform and operands to use.
the operands may come from memory,registers or instruction itself.
instructions are encoded as binary numbers.

`a = b + c;`   `ADD a,b,c`
`ADD` is called mnemonic and indicates what operation to perform. the operation is performed on `b` adn `c` , the source operands and the result is written to `a`, the destination operand.

`SUB a,b,c  ; a = b - c`
comments begin with ; and are only single line till end

the number of instructions is kept small so that the hardware required to decode the instruction and its operands can be simple,small & fast.
more elaborate operations are performed using multiple simple instructions. thus, **ARM** is *reduced instruction set computer* **RISC**.

complex instructions in CISC requires added hardware and overhead that slows down frequently used simple instructions.

#operands

the instruction need physical location from which to retrieve the binary data.
`ARMv8` - 64 bit

ARM uses 16 registers called the *register set* or *register file*
reading from small register file is faster than large memory.
typically made from small SRAM array.
`MOV R5, #0xFF0` - immediates prefixed by `#`

immediates are unsigned 8(numbers) or 12(address) bits numbers.

in ARM, instructions operate exclusively on registers, so data stored in memory must be moved to a register before it can be processed.

ARM uses 32-bit memory addresses and 32-bit data words.
##### Desing Process
we divide our microarchitecture into two interacting parts :
- datapath
- control unit
the datapath operates on words of data.

*multiplexer*
they choose an output from among several possible inputs based on the value of select signal.
![[multiplexer.png|400]]

*decoder*
it has $N$ inputs $2^{N}$ outputs. it asserts exactly one of its outputs depending on the input combination.
![[decoder.png|250]]

*Arithmetic/Logical Unit ALU*
combines a variety of mathematical and logical operations into a single unit.
![[ALU_symbol.png|100]]
N-bit ALU with N-bit inputs and outputs. ALU receives a control signal F that specifies which function to perform.

#memory
physically, RAM is implemented as 2d array with rows containing many columns of bits or bytes. architecturally, cpu sees memory as linear array of byte addressable locations. 
![[memory_array.png|500]]
the decoder asserts wordline, data is read from bits on that wordline through data bitlines

multiported memories can access several addresses simultaneously.

RAM is volatile, ROM is non-volatile

DRAM stores data as a charge on a capacitor. data needs to be refreshed periodically and after a read. wait for charge to move slowly from capacitor to the bit-line.

SRAM stores data using a pair of cross-coupled inverters. no refresh needed.
power is needed in both sram and dram.

SDRAM - synchronous dram, uses clock to pipeline memory access
DDR SDRAM - uses both rising and falling edges of clock to access data.

#register_file

group of registers.
multi-ported SRAM array
![[register_file.png|250]]


the control unit receives the current instruction from the datapath and tells the datapath how to execute that instruction.
it produces multiplexer select, register enable, memory write signals.
![[design_arm_1.png|550]]
![[state_elements_ARM.png]]

#addressing_modes

*register-only addressing*
uses register for all source and destination operands.

*immediate addressing*
uses 16-bit immediate along with registers as operands.

*base addressing*
the effective address of the memory operand is found by adding base address in the register to the sign-extended 12-bit offset found in the immediate field. U-bit decides the sign 0->sub 1->add.

*PC-relative addressing*
conditional branch instructions uses it to specify the new value of PC if the branch is taken.
the signed offset in immediate field is added to PC to obtain new PC.

branch target address, instruction address of the else label.

the processor calculates BTA from the instruction by sign-extending the 24-bit immediate, multiplying by 4(<<2) and adding it to $PC+8$.

the *program counter* is logically part of the register file, it is read and written on every cycle independent of the normal register file operations and is built as a standalone 32-bit register.

its output $PC$ is the address of current instruction and its input $PC^{'}$ indicates the address of next instruction.

the *instruction memory* has a single read port. it takes a 32-bit instruction address input,A and reads the 32-bit data (instruction) from that address onto the read output data (RD)

the 16-element X 32-bit *register file* holds registers R0-R14 and has an additional input to receive R15 from PC.

the register file has 2 read ports and one write ports.
the read ports take 4-bit register address, A1 and A2.
they read the 32-bit register values onto the read data outputs RD1 and RD2.

write port takes a 4-bit register address A3, 32-bit write data input WD3 & a write-enable input WE3 and a clock.

a read of R15 returns the value from the $PC$ $+$ $8$
writes to R15 must be specially handled to update the PC because it is separated from the register file.

the *single-cycle microarchitecture* executes an entire instruction in one cycle. cycle time is limited by slowest instruction. requires separate instruction and data memories, which is unrealistic.
one instruction per cycle but very long cycle.
eg. `ADD` - no data memory access , wastes time on this. coz `LDR` needs data memory access.
we are fetching instruction and accessing data from memory in the same cycle, so we need diff. memories for each.

the *multi-cycle microarchitecture* executes instructions in a series of shorter cycles.
it adds several non-architectural registers to hold intermediate results.
it executes only one instruction at a time but each instruction takes multiple cycles. need only one memory, fetch in one cycle R/W on other.

*pipelined microarchitecture* applies pipelining to the multi-cycle microarchitecture.
increased throughput, non-arch. registers needed.

###### single cycle processor
control unit generates control signals based on the current instruction.

$PC$ contains addr of instr to exec.
read this instr from instr mem

for **LDR**, next step is to read register containing base addr.
this register address is specified in $R_n$ (bits from `19:16` 31 to 0) field of the instr.
these bits of instruction are connected to address input of port of register file A1 , the register file reads this value into RD1 output.
offset is stored in the immediate field of the instruction.
offset is an unsigned value so it must be zero-extended to 32-bits.

this extended 32-bit value is called *ExtImm* and we use separate hardware labelled *extend* for this purpose.
*ImmExt`31:12`* = 0
*ImmExt`11:0`* = *Inst`11:0`*

the processor must add base addr. and offset to find addr. to read from memory.
the ALU receives two inputs `SrcA` and `SrcB` , these comes from register file and extended immediate.
the 2-bit ALU control signal specifies the operation.
![[ALU.png|350]]
the ALU computes several candidate resutls in parallel, and the control signal `F1:0`select final output.
`F2` , controls how the second operand is formed (ADD/SUB)
`F2 = 0` addition $A+B$ `F2=1` subtraction $A+\overline{B}+1$ 


ALU generates an 32-bit ALUResult, this is sent to data memory as a address A to read data onto ReadData bus and written back to destination register. using write port in register file WD3,
address of this register is read from instruction (bits `15:12`) and passed to A3 line of RF, RegWrite control signal is connected to port 3 write enable input WE3 and is asserted during an `LDR` instruction so that data value is written into the register file at the next rising edge (instruction finished, PC is updated, data written to register)

while the instruction is being executed, the processor must compute the address of next instruction. because instructions are 32-bits the next instruction is at PC+4, we can use adder to increment PC by 4.

*power of stored program*
instructions stored in memory.
running a new program doesn't need large efforts or time to reconfigure/rewire hardware, only writing new program to memory.

Executing instruction at address X
Prefetched instructions at X+4 and X+8, coz of fetch,decode,execute
so reading R15 gives (current executing instruction addr + 8) and 
writing R15 sets program counter to that value.

therefore another adder is needed to increment PC by 4 and store PC+8 in R15. (architectural desig, originally ARM was pipelined)

the `PCSrc` control signal is used to decide whether next instr addr will come from `ReadData`(memory, when some instr updates PC/R15) or `PCPlus4`.
![[LDR_ARM.png]]

**STR**
write enabled port for read memory is controlled by `MemWrite`.
data is still read from memory but `ReadData` is ignored because   `RegWrite` = 0.
for STR data is read from `Instr 15:12` $R_d$
![[instruction_format_arm.png|350]]
the `cond` field specifies the condition under which an instruction executes, based on CPSR flags; if the condition fails, the instruction performs no state update.

bits `27:26` , instruction class
00 -> data processing
01 -> ldr/str
10 -> branch
11 -> interrupt
![[operand2_arm_i=0.png|350]]

| 00  | ADD |
| --- | --- |
| 01  | SUB |
| 10  | AND |
| 11  | ORR |
ALUControl

ALU produces four flags
ALUFlags (Zero,Negative,Carry,oVerflow)

data processing instructions only use 8-bit immediate rather than 12-bit for address offset immediate. so we provide `immSrc` control signal to extend block.

for register addressing, the second source register is from $R_m$, specified by $Instr_{3:0}$ rather than from immediate.
we must add multiplexers on the inputs of register file and ALU to select this second source register.
![[branch_single_cycle_ARM.png]]

instruction class determines behaviour, control unit checks it.

NOP - instruction executes but no effect on processor state. used to avoid hazards.

for data processing instructions second source register in register addressing in chosen from `Instr 3:0` 

`opcode` - ALU control logic
`S` - $1$ -> update flags N,Z,C,V , $0$ -> flags unchanged

$R_d$ `15:12` address is passed to control unit, so it can decide to read R15 or `19:16` and in case of STR read data from `15:12` or `0:3` in case of register addressing.

*control unit*
it is divided into two parts :
- decoder
- conditional logic

decoder determines the type of instruction class.
![[ORR_on_ARM.png]]

![[LDR_critical_path.png]]

###### Multicycle Processor
PC is connected to the address input of memory. the instruction is read and stored in new non-architectural *instruction register(IR)*
so that it is available for future cycles. The IR receives an enable signal, called IRWrite, which is asserted when the IR should be loaded with new instruction.

the register file reads into RD1, which is stored in A , another non-arch register.
ALUresult is stored in ALUout.

we add a mux in front of memory to choose Adr from wither PC or ALUout
based on `AdrSrc` control line.
the data read from the memory is stored in `Data` register.

instead of connecting Data register directly to WD3, we add a mux on ALUresult to choose either `Data` or `ALUout`. coz some instructions need to read data from memory and store in a register and others want to store ALUout in register.

we use ALU to increment PC, we add two mux, one to choose b/w PC and RD1 and another to choose between extender and 4.
![[complete_multicycle_processor.png]]
###### pipelined
![[pipelined_ARM.png|550]]

the length of pipeline stage is set by the slowest stage, the memory access(in the fetch or memory stage)
because the stages are not perfectly balanced with equal amounts of logic, the latency is longer for the pipelined processor than for the single-cycle processor.

a central challenge in pipelined systems is handling hazards that occurs when result of a instruction is needed by a subsequent instruction before the former instruction has completed.

the register file in the pipelined processor writes on the falling edge of CLK so that it can write result in the first half of the cycle and read that result in the second half of the cycle for use in a subsequent instruction.

In the basic pipelined design, the PC is incremented by 4 in the Fetch stage and the result (PC+4) is written both back to the PC and into the Fetch–Decode pipeline register. In the next cycle, when the same instruction reaches the Decode stage, this stored value is incremented by 4 again to form PC+8, which is needed for ARM’s PC semantics. However, since the PC itself has already been incremented in the previous cycle, the value computed as PC+4 in Fetch for a given instruction is logically equivalent to the PC+8 value needed for that same instruction in Decode. By forwarding PC+4 from Fetch and treating it as PC+8 in Decode, the design eliminates the need for an extra pipeline register and a second 32-bit adder, saving hardware without changing behavior.

In the pipelined ARM datapath, this is a timing trick to avoid a read–after–write hazard without extra forwarding. The register file is designed so that writes occur on the falling edge of the clock, while reads are combinational and occur during the rest of the cycle. As a result, when one instruction reaches the Writeback stage and writes its result on the falling edge, a following instruction that is in the Decode stage during the same cycle can immediately read the updated value in the second half of that cycle. This allows a value produced by one instruction to be written and then read by the next instruction without stalling the pipeline, even though both actions happen in what is logically the same clock cycle.

The error is in the register file write logic, which should operate in
the Writeback stage. The data value comes from ResultW, a Writeback
stage signal. But the write address comes from InstrD15:12 (also known
as WA3D), which is a Decode stage signal. In the pipeline diagram of
Figure 7.43, during cycle 5, the result of the LDR instruction would be
incorrectly written to R5 rather than R2.
Figure 7.45 shows a corrected datapath, with the modification in
black. The WA3 signal is now pipelined along through the Execution,
Memory, and Writeback stages, so it remains in sync with the rest of
the instruction. WA3W and ResultW are fed back together to the register
file in the Writeback stage.
![[pipelined_with_control.png]]

if cond is *false*, the condition unit outputs `CondEx=0`, and instruction behaves like $NOP$ .
no branch,reg,mem,flag write control signals are disabled by ANDing them with CondEx.

update PC if instruction wants to update it or it is a branch instr.
that why we are doing (PCsrc AND CondEx) OR (Branch AND CondEx)

