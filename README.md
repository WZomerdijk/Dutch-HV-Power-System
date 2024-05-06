# Pandapower - Open Data Based Model of the Dutch HV Power System in 2021
A numerical model of a power system can be used to get accurate insights into the impact of policies and investment decisions regarding the transformation of the energy system, while also helping in identifying bottlenecks in implementing decisions. This GitHub page presents a valid model of the Dutch high-voltage power system based on open data and open-source software. The representative model enables interdisciplinary research on policy-making and investment decisions specific to the Netherlands.

## Extra information
The work underlying these models of the Dutch HV power system was performed in 2021 and the corresponding models have not been updated since then.  
The presented models are generated in pandapower and exported as pickle files.  
The models are tested on python version 3.10.13, pandapower version 2.13.1, and pandas version 2.1.1.

## Description of models
The models can run power flow and optimal power flow for a balanced AC power system.  
Thus, three phase power flow is currently not possible.  
The models can be loaded by using the function: 
```
pandapower.from_pickle(net, ...)
```

### grid_2021.p
110/150/220/380 kV Dutch Power System in 2021. Includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower).

<img src="https://github.com/WZomerdijk/Dutch-HV-Power-System/assets/122889461/aa727cb4-dda8-497a-9280-7ece9b2b6df7" width="500">

### aggregated_grid_2021.p
220/380 kV Dutch Power System in 2021. Includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower).

<img src="https://github.com/WZomerdijk/Dutch-HV-Power-System/assets/122889461/84bc4cb4-692b-455f-9133-3fd849f18960" width="500">

### aggregated_grid_2021_with_generators_costs.p
220/380 kV Dutch Power System in 2021. Includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower). Moreover, all generators are mapped to their corresponding buses and the operating costs are provided per generator type (for OPF).

## Simulation of models
The last model was slightly adjusted to run a simulation for 2018. The simulation file is provided as *aggregated_grid_2018_with_generators_loads_costs_controllers.p*.  
This is the 220/380 kV Dutch Power System in 2018. It includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower). All generators are mapped to their corresponding buses and the operating costs are provided per generator type (for OPF). Furthermore, the aggregated loads are mapped to corresponding buses and controllers are implemented to run an hourly analysis on the entire Dutch power system for 2018.  
As this model is developed for analysis in 2018, generators that were not commissioned before 2018 were excluded.

### aggregated_grid_2018_with_generators_loads_costs_controllers.p
All sustainable generators (solar and wind) and loads have controllers that provide them with an hourly generation/consumption setpoint. For running an optimal power flow, the operating costs per generator type per hour in 2018 are also provided by the controller. These prices are based on the fuel prices, generator efficiencies, and the ETS prices for carbon emmissions.  
The entire simulation can be ran by following these steps (provide an input for *time_steps*):  
```
pandapower.timeseries.run_timeseries(net, time_steps=[], continue_on_divergence=True)
```

## References
For information on and questions related to pandapower datastructures/functions/etc. you can check the support page of [pandapower](https://pandapower.readthedocs.io/en/v2.13.1).  
For information on the underlying work and the open data sources, you can check the corresponding [conference paper](https://ieeexplore.ieee.org/document/9960703).  
For extra information on the underlying work and examples of analyses supported by these models, you can check the corresponding [MSc thesis](http://resolver.tudelft.nl/uuid:b9d69d1b-d2bf-4ce0-acc6-97171cde3568).
