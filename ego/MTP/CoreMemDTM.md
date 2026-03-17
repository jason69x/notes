existing DTM policies address processors and 3D memories independently, which causes overcompensation.

existing CPU DTM policies slows down heated cores signicantly increasing overall execution time.

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



