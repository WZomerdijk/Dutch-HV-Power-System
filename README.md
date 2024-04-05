# Open Data Based Model of the Dutch HV Power System in 2021 
A numerical model of a power system can be used to get accurate insights into the impact of policies and investment decisions regarding the transformation of the energy system, while also helping in identifying bottlenecks in implementing decisions. This GitHub pages presents a valid model of the Dutch high-voltage power system based on open data and open-source software. The representative model enables interdisciplinary research on policy-making and investment decisions specific to the Netherlands.

## Extra information
The work underlying these models of the Dutch HV power system was performed in 2021 and the corresponding models have not been updated since then. 

The presented models are generated in pandapower and exported as pickle files.
The models are tested on python version 3.10.13, pandapower version 2.13.1, and pandas version 2.1.1.

## Description of models

grid in 2021


the loads can be fixed
the gen are flexible and controlled via opf or any random setting between min and max
the sgen (for solar and wind) can not be controlled and are adjusted per timestep through controllers
generators that are included but are not in service are still connected to the grid, but at the moment of coding temporarily disconnected (due to maintenance, high operating costs, etc.)

## Description of capabilites
The model can run power flow and optimal power flow for a balanced AC power system.
Thus, three phase power flow is currently not possible.

## Description of simulation file
part of the 2018 year can be run by using the timeseries function within pandapower
simulation is performed for 2018 due to limited data availability
import pandapower.timeseries as timeseries
timeseries.run_timeseries(net, time_steps=[0,1,2,3,4,5,6,7,8,9], continue_on_divergence=True)
timeseries.run_timeseries(net, time_steps=ts, continue_on_divergence=True)

## References
For information on and questions related to pandapower datastructures/functions/etc. you can check the support page of pandapower : https://pandapower.readthedocs.io/en/v2.13.1

For information on the underlying work and the open data sources, you can check the corresponding conference paper : https://ieeexplore.ieee.org/document/9960703

For extra information on the underlying work and examples of analyses support by these models, you can check the corresponding MSc thesis : http://resolver.tudelft.nl/uuid:b9d69d1b-d2bf-4ce0-acc6-97171cde3568
