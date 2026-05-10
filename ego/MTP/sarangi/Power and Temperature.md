FLOPS : Floating Point Operations per Second

wires and transistors can damage on high cpu temperature

probability of failure is an exponential function of the die temperature.

consistent high temperature increases the probability of the battery of the mobile phone exploding.

as temperature rises, more electrons gain enough energy to cross barriers -> leakage increases exponentially

leakage power $\propto$ temperature

resistance of materials increases with temperature
higher temperature -> higher resistance -> more power dissipated as heat

dynamic power increase -> temperature increase -> leakage power increase -> heat generated -> temperature increase

due to quantum and thermal effects electrons can still move even when the transistor is off.

work = process (energy transfer)
energy = state (ability to do work) (kinetic/gravitational potential)

there is cyclic dependence between power and temperature.

electrons collides with atoms in the lattice, this vibrates atoms and this vibration (kinetic energy) is heat. 

electrons are moving, so they have kinetic energy

voltage - energy per electron
current - no. of electron per second

in most cases, power and temperature dependency converges
else thermal runaway, need to shutdown system immediately
system stabilizes when heat generated = heat dissipated (cooling)

*power dissipation*
- dynamic 
- leakage

dynamic power is the power consumed due to charging and discharging of capacitances during switching activity.

leakage power accounts for roughly 20-40% of the overall CPU power budget.
even though leakage power per transistor is magnitude lower than dynamic power, they become significant when we consider cumulative sum across billions of transistors.
- PN junction reverse bias current
- subthreshold leakage
- gate oxide tunneling,thickness of gate oxide is reducing, this increases electric field across the gate oxide. this causes quantum tunneling. electrons in the channel can directly escape into the gate oxide. this leads to leakage current. coz in an ideal transistor gate oxide is assumed to be a perfect insulator.
- hot carrier injection
- gate-induced drain leakage
- punchthrough current, depletion layers may combined, if the channel is short.


conductors, few electrons in outermost (valence) shell, so they are loosely bound to the nucleus, they can easily become free electrons and move through the material, allowing current to flow.

insulator more electrons, tightly bound to nucleus
semi-conductors, 4 valence electrons, controllable conductivity

![[transistor_atomic.png|400]]


current, charge flow per unit time


*semi-conductors*

![[semi_conductors.png|500]]

charge carriers,
- electrons (n-type)
- holes (p-type)

![[depletion_layer.png|500]]

right at the boundary (PN junction), extra electrons from n-type will fill holes in p-type, leaving extra protons in n-type making it positively charged and aggregating extra electrons in p-type making it negatively charged. after some time, the middle portion (depletion layer) has no charge carriers, electrons from n-type needs more energy to cross this barrier.
no charge carriers, acts as a insulator

apply voltage $\gt 0.6$ to break the barrier.

![[diode.png|600]]


![[capacitor.png|300]]

battery has a high concentration of electrons (potential)
when it is connected to two metal plates separated by a di-electric
when battery is turned on, electrons from +ve side metal plate attracts towards +ve terminal of battery so this plate becomes +vely charged and electrons flow from -ve terminal of battery to the metal plate connected to it, so this plate becomes -vely charged,this continues till potential difference between capacitor and battery becomes same.
if we now connect a bulb to capacitor, since electrons can't flow through dielectric, they flow around lighting up the bulb to the other side.

time constant (RC delay), 
tells us how fast a capacitor charges/discharges
$\tau = RC$ 
charging: after time $\tau$, capacitor reaches ~63% of its final voltage.
discharging: after time $\tau$, capacitor drops ~37% of its initial voltage.

RC delay increases when charge has more resistance to flow through or more capacitance to fill. 


to make analysis simpler and simulation faster with some loss of accuracy, we replace every circuit element with an equivalent RC circuit made of just resistors and capacitors.

whenever we have two metallic conductors in proximity, they will act like a classic capacitor - two parallel plates separated by a dielectric. voltage difference between these two wires creates electric field. this field stores energy -> capacitance.

delay dominated by moving charge into capacitances through resistances.

inductance is the tendency of a circuit to resist changes in current by generating an opposing voltage due to changing magnetic fields.

*RC model*

there is some amount of dynamic power dissipation because of the passage of current through resistive elements in the circuits and in the devices themselves.

encountering a resistance, power dissipated $I^2R$.

whenever there is a current flow in a circuit to effect a transition in the voltage level (logical 0 -> 1 or 1 -> 0), there is some amount of dynamic power dissipation because of the passage of current through resistive elements.
hence, to compute dynamic power dissipation, we need to locate all the voltage transistions in a cycle, compute the current flow required to effect the transitions, and then compute the sum of resistive ($I^2R$) losses throughout the circuit.

we only care about locations of all the voltage transitions in the circuit while computing dynamic power.

$I = \frac{V}{R}e^{-\frac{t}{RC}}$ 

$P_{tot} = \int\  V\times I$

total energy provided by the voltage source in a typical charging cycle $P_{tot} = CV^2$ .
out of this energy, some of it is dissipated as heat via the resistor 
($\frac{1}{2}CV^2$) and the rest is stored as energy across the capacitor ($\frac{1}{2}CV^2$).

when capacitor is discharged energy is also dissipated as heat ($\frac{1}{2}CV^2$)

total energy dissipated in charge-discharge cycle is ($CV^2$)

given that ($\frac{1}{2}CV^2$) units of energy are dissipated in every cycle (charging or discharging), the power consumption is equal to ($\frac{1}{2}CV^2/t$)
where $t$ is cycle time.


$\therefore$ dynamic power consumption $P_{dyn} \propto CV^2f$

since all the transistors may not be switching their state in a given cycle, we need a activity factor (0 - 1) $\beta$

$P_{dyn} \propto \beta CV^2f$

DVFS, dynamic voltage and frequency scaling

changing power consumption by tuning the voltage and frequency. modern processors uses a table that contains few voltage and frequency settings, (DVFS settings). processor is only allowed to operate in any one of these settings.

$E = P\times t = P\times D$, where $D=1/f$ is delay

we use $ED^2$ metric because it removes bias.
we can use this metric to compare designs, because it remains constant with respect to any DVFS scaling.
it is an inherent property of an architecture.

better $ED^2$ means better design in terms of speed and energy combined


a typical inverter has two transistors: pull-up PMOS transistor and pull-down NMOS transistor.
when it is going through an input transition there is a brief period when both the transistors are conducting. during this time there is a short circuit between the supply and the ground. the corresponding current that flows is known as the short-circuit current and corresponding power is short-circuit power.

real devices leak some power via interfaces that are assumed to be perfect insulators.

thermal interface material (TIM) ensures proper conductive heat transfer between the structures. thermal paste between chip and the spreader or the spreader and heat sink. gel like material to ensure that it has good contact with solid structures that it connects.

steady-state analysis, temperature profile after it has been stabilized
transient analysis, compute variation of temperature over time

conduction, 
heat transfer between objects that are in direct contact. transferred from high temperature object to low temperature object.

convection,
heat transfers occuring due to fluids.
hot air moves upwards. molecules move faster, air expands, same mass larger volume, density decreases. buoyancy pushes it upwards. denser fluids (cold air) pushes lighter fluid upward.
fans are places to provide cold air that pushes hot air from processor.

radiation,
heat transfer in free space. any heated objects emits radiation. 
this radiation is absorbed by remote objects, this increases their temp


Managing Leakage Power:

power gating,
stop the flow of current into the circuit. no leakage. no power dissipated.

- disconnet supply voltage
- disconnet connection to ground
- or both

voltage is always measured with repect to some other voltage. there is no thing as absolute voltage. voltage is difference in potential energy

- current available to functional unit should not reduce after adding sleep transistors. to carry required current these transistors have to be large.
- large transistors take long time to switch on and off. this puts limit on power gating frequency 
- when we enable/disable power gating, current flow changes abrubtly in power and ground grids. as per lenz law, any electrical conductor opposes the change in current through it by inducing a back EMF (power gating noise). need to add large decoupling capacitors. they also increase leakage power.
- sleep transistors also have a potential difference across their terminals. supply voltage needs to be increased to compensate for decreased effective voltage supply to functional unit.
- functional unit loses its state after it is power gated.
- predicorts needed to know when functional unit is not expected to be used. else wasted cycles in waking FU from sleep.
- dynamic power consumption increases coz cycling between active and sleep states is associated with drawing large currents from source.

change $V_{th}$,
we can slow down transistors on non-critical path. lets say critical path takes 300ps to propogate signal. if we have a path which is 200ps, we can slow down transistors in this path. such that length of this path becomes 300ps. by slowing down transistor we save on leakage power. make threshold voltage higher for these transistors.
threshold voltage is the minimum gate-to-source voltage at which a mosfet starts forming a channel so current can flow.
lower the threshold voltage, higher the leakage, faster the transistor.


sometimes temperature hotspot can form if a functional unit is being heavily used. this can cause local temperature rise. throttle or move computation.

managing dynamic power,

drowsy mode, 
supply voltage is reduced such that the stored value can be maintained, but cell cannot be accessed. drowsy cache.

clock gating, 
disable clocks to unused functional units.
we are not allowing latches that feed data to the functional unit to change their values. no voltage transitions in the inputs of FU, thus no current flow or resultant dynamic power dissipation.
deterministic clock gating is preferred, avoids delay issues of non-dtr

DVFS,
to increase frequency, we increase supply voltage by programming voltage regulators on the motherboard. this takes time.
after this we pause execution and re-tuen the PLL (phase locked loop) based clock generator of the processor to generate new high-frequency clock signal.

to decrease frequency, we first pause the system, relock PLL to new frequency. after that voltage regulators gradually reduce the voltage.

we can apply dvfs depending on the use/application. takes $10-20\micro s$

for 3d chips we can use microchannel based cooling. small tube within the chip that carrier coolant. cools inner layers of 3d chip.
job placement.


we can replace a thermal problem with an analogous electrical problem, and then we can use traditional circuit simulators to find the voltages and currents at different places. these values can then be mapped to temperature and power values.

