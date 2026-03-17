customizable open-source HBM controller

- fine-grained memory command scheduling
- dynamic bandwidth allocation
- workload-specific optimization
- compatibility with FPGA-based architectures

**FPGA** 

field-programmable gate array is a flexible integrated circuit that can be reprogrammed after manufacturing to perform custom digital logic functions.

fpga operates by configuring its internal logic and interconnects via a hardware description language (HDL), such as VHDL or Verilog.
configutation is compiled into a bitstream, which programs the FPGA.
once programmed the FPGA executes logic in parallel enabling low-latency and high-throughput.

it consists of 
1. Configurable Logic Blocks (*CLB*), each CLB contains:
   - LUTs (look-up tables) - implements combinational logic
   - Flip-Flops - store state

2. programmable routing network - wires everything together
3. i/o blocks
```verilog
assign y1 = a & b;
assign y2 = c | d;
assign y3 = e ^ f;
```
these 3 logic equations are mapped to 3 different LUTs


###### HBM - High-Bandwidth Memory
significantly higher bandwidth and energy efficiency.

propreitary hbm-controllers hinders explorations of new optimizations.

HBM comprises multiple DRAM dies - typically four - vertically stacked above a base logic die, which functions as the interface to external components.
![[hbm_stack.png|500]]

interconnection between the DRAM dies and the logic die is enabled by *Through-Silicon-Vias* (TSV).
each DRAM die is partitioned into two independent memory channels, with each further subdivided into multiple DRAM banks 8 or more


*NeuroMap Idea* - statically map cores to HBM channels and dynamically map DNNs tasks to cores


the charge stored on each capacitor is too small to be read directly and is instead measured by a circuit called a sense amplifier.
the sense amplifier detects the minute differences in charge and outputs the corresponding logic level.
reading the bitline forces the charge to flow out of capacitor.
precharging is performed to put value read back into capacitor.
capacitors leak data overtime and needs to be refreshed.

a rank is a separately addressable set of DRAMs.

----

**dram**
![[dram.png]]


*cell*
stores 1-bit of information. consists of a capacitor that holds the charge and a transistor acting as a switch.
since the capacitor discharges over time, the information eventually fades unless the capacitor is periodically REFRESHed. 
![[dram_cell.jpg|400]]
source, gate, drain

*rows, columns, banks*
memory cells are arranged in a grid of rows and columns, known as bank.
![[dram_bank.png|280]]

memory bank is like a book, row is page no. , col is line number

- ACTIVATE: row address decoder activates row number specified in address and brings data into a structure called *sense amp*.

- COLUMN STORAGE: then a column address decoder extracts data from a specified column and streams it out.

- PRECHARGE: after access, row has to be closed and data in sense amp is returned back into row cells.
<video src="bank_access.mp4" controls width="400" height="400"></video>

in real DRAMs, row width is in thousands, so we need column decoder fs

there is a significant penalty to page misses since there are many more ACTIVATEs and PRECHARGEs to be performed.
![[page_miss_dram.png|500]]

*memory die*

is one physical wafer and is made of a collection of banks.
different densities depending on no. of rows, cols & banks.

a *physical wafer* is a thin, circular slice of highly purified semiconduction material (silicon) on which integrated circuits are fabricated. 

*memory channels*

the interface to the memory is like a pipe. a bundle of wires. this bundle has a name - *Command-Address(CA)* and *Data(DQ) bus*

one end we have logic die with memory controller and other memory die.
this is pipe is called a *channel*.
![[channels_dram.png|500]]

*memory package* is made of multiple channels, each channel is connected to a separate memory controller and works independently.
![[package_dram.png|500]]

performance depends on:
- how many pipes
- size of each pipe
- data flow speed of pipe

well-structured storage and retrieval patterns can significantly improve utilization.

----------------------------------------------------------------------------

**dram**
![[dram.png]]


*cell*
stores 1-bit of information. consists of a capacitor that holds the charge and a transistor acting as a switch.
since the capacitor discharges over time, the information eventually fades unless the capacitor is periodically REFRESHed. 
![[dram_cell.jpg|400]]
source, gate, drain

*rows, columns, banks*
memory cells are arranged in a grid of rows and columns, known as bank.
![[dram_bank.png|280]]

memory bank is like a book, row is page no. , col is line number

- ACTIVATE: row address decoder activates row number specified in address and brings data into a structure called *sense amp*.

- COLUMN STORAGE: then a column address decoder extracts data from a specified column and streams it out.

- PRECHARGE: after access, row has to be closed and data in sense amp is returned back into row cells.
<video src="bank_access.mp4" controls width="400" height="400"></video>

in real DRAMs, row width is in thousands, so we need column decoder fs

there is a significant penalty to page misses since there are many more ACTIVATEs and PRECHARGEs to be performed.
![[page_miss_dram.png|500]]

*memory die*

is one physical wafer and is made of a collection of banks.
different densities depending on no. of rows, cols & banks.

a *physical wafer* is a thin, circular slice of highly purified semiconduction material (silicon) on which integrated circuits are fabricated. 

*memory channels*

the interface to the memory is like a pipe. a bundle of wires. this bundle has a name - *Command-Address(CA)* and *Data(DQ) bus*

one end we have logic die with memory controller and other memory die.
this is pipe is called a *channel*.
![[channels_dram.png|500]]

*memory package* is made of multiple channels, each channel is connected to a separate memory controller and works independently.
![[package_dram.png|500]]

performance depends on:
- how many pipes
- size of each pipe
- data flow speed of pipe

well-structured storage and retrieval patterns can significantly improve utilization.


*precharge* is the operation that restores bit-lines to a neutral reference voltage ($V_{dd}$) after a read or write, preparing the memory array for next access.

if page hit (using same row) then no need for precharge (just activate once & perform accesses and close i.e precharge after) and hence saved time, else need to precharge before accessing different row and then activate.

$BL$ and $\overline{BL}$ are just to help sense-amplifier read the stored value,
finally $BL$ is passed on data lines

the feedback loop empties/refills both capacitor and $BL$ at the same time, thereby storing back the charge, lets say $BL$ tends to 0, then cross-coupled inverted will make $\overline{BL}$ 1 while making this 1 the capacitor is still connected to $BL$, thus it makes both the capacitor and the bitline 0.

access transitor, connects capacitor to bit line, acts as a gate

row buffer is used as a cache to increase hit rates.
size of sense amplifier is 100x size of cells

![[DIMM.png|500]]

In a 64-bit DRAM channel using ×8 chips, memory module uses 8 DRAM chips, and each chip contributes 8 data bits in parallel. Each chip internally has the same bank structure and receives the same command/address signals from the memory controller. So when the controller issues ACTIVATE with a bank address and row address, that command goes to all chips simultaneously. 
If the controller specifies *bank 0*, then *bank 0 in every chip* is activated, and the *row decoder inside each chip selects the same row number* in its own bank 0. As a result, each chip loads that row into its row buffer. Later, when the controller issues a READ with a column address, the column decoder inside each chip selects the same column group from its row buffer. 
In a **×8 chip**, that column group corresponds to 8 bits (because the chip’s I/O width is 8). These 8 bits from each chip are then driven onto that chip’s 8 data pins. Since there are 8 chips operating in parallel, the system receives 8 bits × 8 chips = 64 bits in one transfer on the memory channel. 
banks are independent inside each chip, but across chips they operate in lockstep for a given command, because all chips share the same command/address bus; this is why the module behaves like a single 64-bit wide memory device even though it is physically composed of multiple narrower chips.

![[dram_channels.png|500]]

front of dimm rank 0, back rank 1

channels matters to increase bandwidth. they have seperate data bus
dual channel, quad channel

one rank activates at a time, but more ranks give more capacity
chip select is used to select a rank.

![[dual_channel_dram.png|400]]

 

