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
an n-bit register is a bank of n flip-flops that share a common CLK input, so that all bits of the register are updates at the same time.

the output of *combinational logic* depends only on the current input values.
the out of *sequential logic* depends on both current and prior input values.

bistable element, two stable states.
metastable, third possible state between 0 and 1.

an element with N stable states converys $log_{2}{N}$ bits of information.

when the power is first applied to a sequential circuit, the initial state is unknown and unpredictable. eg. cross coupled inverters
![[cross_couples_not.png|250]]
user control is needed.
latches and flip-flops provide inputs to control the value of the *state variable*.
###### SR Latch
![[SR_LATCH.png|250]]
cross coupled NOR
![[SR_table.png|250]]

