existing DTM policies address processors and 3D memories independently, which causes overcompensation.
need to coordinate DTM of two subsystems.

existing CPU DTM policies slows down heated cores signicantly increasing overall execution time and performance overheads (reduced frequency, thread migration, stalling, reduced parallelism)

distributed cooling, place fan for each sub-system instead of a large fan for full system. 

thermal management incurs performance and energy overheads
$E = P\times T$ ,
lowering freq/volt reduces power but increases execution time, which may increase energy consumption.

neglecting core and memory temperature dependencies leads to additional overheads. 

cores have a base clock,
core frequency = $BCLK\times multiplier$
reduce multiplier to reduce freq.

VRM, voltage regulator module

decreasing performance, lower freq. then voltage
increasing performance, incre volt. then freq.

multiples cores can access the same channel, but the channel physically serializes transfers.

*stall-balanced core DVFS policy*

core idea,
if a core is frequently stalled (eg, waiting on memory, cache misses), increasing its frequency won't improve performance much - it just wastes power. if a core is mostly compute-bound (few stalls), higher frequency gives real performance gains. the policy tries to balance these stalls across cores while minimizing energy.

instead of scaling all cores equally, 
- reduces frequency for stall-heavy cores -> saves power with minimal slowdown
- increases frequency for compute-bound cores -> boosts performance efficiently

equalize or balance effective progress across cores.

**DVFS** - dynamic voltage and frequency scaling

power management technique where a processor dynamically adjusts its clock frequency and supply voltage based on workload demand to balance performance and enery efficiency.

higher frequency requires higher voltage

$P\propto V^2\cdot f$ , power consumption

even a small drop in voltage gives large power savings.

DVFS is about running just fast enough for the workload, not faster, to save power without hurting performance too much.

*CoreMemDTM* is built upon the premise that due to inter-dependence between core and memory, a DTM decision taken for one of them might alleviate the temperature for the other.

CoreDTM - DVFS , lower frequency and voltage
MemDTM - low power states (idle, power down, self-refresh)


thermal slack, how far a component is from its thermal limit
$thermal slack = T_{max} - T_{cur}$

small slack -> danger , large slack -> safe

if both are hot, choose component with lower thermal slack

multi-level slack-balanced DVFS,
adjust frequency based on how much slack is left

while prior works focus on reducing the total energy consumption of core and memory, joint thermal management of core and memory has not been considered earlier.

consider a multi-core system with a 3D main memory executing various compute-intensive and memory-intensive applications, which cause significant power dissipation in cores and memory, leading to heating and frequent activation of the corresponding DTM policies independently

DTM policy for core slows down the heated (compute-intensive) cores while memory DTM policy reduces the memory temperature by slowing accesses to heated (memory-intensive) channels.
this in turn slows down the cores executing memory-intensive applications, reducing core temperature further and causing overcompensation as the DTM for core did not account for the additional core stalling (and cooling) due to memory DTM.

to reduce interference at the memory, we consider that each core accesses data of its corresponding 3d memory channel.
compute-intensive - core 0,3
memory-intensive - core 1,2

cores 0,3 heats up and memory channels 1,2 heats up
core DTM slows 0,3 and memory DTM slows 1,2 channels
since cores fetch data from memory, slowing down channels is basically slowing down cores 1,2 therefore, all four cores are slowed.

![[baseline_DTM.png|500]]

*coreDTM*

this policy is invoked at regular intervals (epochs), every kth epoch
this k may change overtime

DVFS frequency for the core is decided based on its maximum temperature in the current epoch.
the number of cores for stall balancing is determined based on the maximum and average temperatures of all cores in the current epoch.

first obtain max temp of each core $T_{DVFS}$ and max temp of all cores $T_{MAX}$

if max temp exceeds safe limit $T_{P}$, place all cores in low power state and stall them.
otherwise DVFS policy for each core is executed which sets freq/voltage
of based on $T_{DVFS}$ and $T_{P}$. after computing this temperature slack it determines the core frequency using a table.

$T_{P}$ - safe temperature limit of processor
$T_{DVFS}$ - max temperature of a core in current k-epoch interval
$T_{MAX}$ - max temperature of all cores in current epoch

we also determine the number of cores (S) for stall balancing using $T_{max}$, $T_{avg}$, $T_p$, inside get_SB_count function. S is larger if $T_{max}$ is close to $T_p$.

S is assigned one of the 3 values $S_2>S_1>S_0$ depending upon difference between $T_{max}$ and $T_p$ and predifined constants $\theta_2\ \theta_1$.

$T_{max}$ can be high even if one or a few cores have high temperatures. in such cases, since many cores are at a lower temperature, we could reduce the aggressiveness of stall balancing.

if $T_{max}$ is larger than $T_{avg}$ by $\alpha$ (indicating only a few cores are heated), the stall balancing core count is decreased by $\beta$.

once $S$ is identified, we reduce the frequency of $S$ cores by one level, in a rotating manner, starting from core that was last considered for stall balancing.

stall balancing is invoked at every epoch,
multi-level DVFS is invoked at every kth epoch.

if $T_{max}\geq T_p$ , put all cores in low_power mode and set frequency to lowest level. else set core state to active.


*memDTM*

places heated memory channels (and their neighbours) in low power state to reduce leakage power dissipation.
during low power state, the memory data is retained, eliminating data migration overheads and the need to have a backup memory.
data is inaccessible during low power state, and the respective cores need to stall until the channels return to the active state.
dynamic power (read/write), leakage power (idle, cooler -> less leakage)

$T_{M}$ - safe temperature limit for memory

memDTM runs every epoch, placing certain no. of channels in low power state. $L_1,L_2,L_3,L_4$ number of channels are placed in low power state when the memory temperature crosses thresholds $\theta_1,\theta_2,\theta_3,\theta_4=T_M$
when the temperature exceeds $\theta_i$ , we select $L_i$ channels for placing in low power.

obtain list of channels in decreasing order of temperature.
place the first channel from this list and its hottest neighbour in low power state.

![[corememdtm.png|500]]

stall balancing,
make cpu and memory wait equal time instead of one being fast and other being slow coz wasted power.

