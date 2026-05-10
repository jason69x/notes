designing cooling solutions for worst-case power dissipation is expensive.
chips that can autonomously modify their execution and power dissipation characteristics permit the use of lower-cost cooling solutions while still guaranteeing safe temperature regulation.

resistance, $R = \frac{\rho\cdot l}{A}$ 

current, charge passing per unit time, $I = \frac{q}{t}$ 

charge creates electric field. 
proton, electron create same electric field but different directions.

power metrics are poor predictors of temperature, sensor imprecision has a substantial impact on the performance of DTM.

temperature depends on how heat flows and accumulates, not just how much is generated.
even if power spikes briefly, temperature may barely change because heat hasn't accumulated yet.

heat flows to neighbouring regions and layers. a units temperature depends on its own power, neighbor's power, past history

power is local, temperature is coupled

leakage power increases with temperature (feedback loop), material properties changes with temperature

thermal diffusion, heat spreading from hotter to colder regions

low lateral thermal resistance -> heat flows easily from neighbors -> strong interaction between blocks

high lateral thermal resistance -> heat stays localized -> weaker interaction

localized heating occurs much faster than chip-wide heating, since power dissipation is spatially non-uniform across the chip, this leads to hot spots and spatial gradients that can cause timing errors or physical damage.

a package designed for the worst case is excessive.

the response by the chip provides the additional cooling and worst-case protection that is needed for reliability.

designing the thermal package for the "worst typical" application rather than the true worst case and using DTM permits reduction in *thermal design power (TDP)*.
for application that do engage DTM, some performance loss may be incurred, because reducing temperature typically entails reducing power density, which in turn typically entails slower execution.
but per-chip savings can be as high as hundreds of dollars.

improving DTM desing will allow greater cost savings with minimal performance cost.

TDP - cooling system is designed to handle at least this much heat.

if we assume worst-case program, then TDP will be high and need expensive cooling system.

clock gating, turn off clock to unused blocks

*using equivalent RC circuit*

heat flow -> current
temperature difference -> voltage

thermal capacitance, to capture the time required for a mass to change temperature.

capacitors stores energy in the form of electrostatic charges.
electric force between these charges keeps them together.

current and heat flow are described by exactly the same differential equations for a potential difference.
these equivalent circuits are known as compact models.

in microarchitectural unit, heat conduction to the thermal package and to neighbouring units are the dominant mechanisms that determine temperature.

each unit on the chip is modeled as a heat (power) dissipator; each underlying region of silicon is modeled as part of the RC circuit, with several RC elements representing lateral and thermal heat flow.
package and convection contributes additional RC elements.

the model is a simple library that provides an interface for specifying some basic information about the package and for specifying any floorplan of any desired granularity. HotSpot then generates the equivalent RC circuit automatically, and, supplied with power dissipations over any chosen time step, computes temperatures at the center of each block of interest.

floorplan - how chip blocks are arranged.

the equivalent circuit is designed to have a direct and intuitive correspondance to the physical structure of a chip and its thermal package.
the RC model therefore consists of three vertical conductive layers of die, heat spreader and heat sink and a fourth vertical, convective layer for the sink-to-air interface.

each layer consists of a vertical RC pair from the center of each block down to the next layer and a lateral RC pair from the center of each block to the center of each edge.

![[hotspot_rc.png|500]]

thinning adds to the cost of production but reduces localized hotspots by reducing the amount of relatively higher-resistance silicon and getting heat-generating areas closer to the higher-conductivity metal package.

