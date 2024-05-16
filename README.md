# Pandapower - Open Data Based Model of the Dutch HV Power System in 2021

![Static Badge](https://img.shields.io/badge/MADE_WITH-PYTHON-orange?style=for-the-badge)

[![python](https://img.shields.io/badge/python-3.10.13-blue.svg)](https://www.python.org/)
[![pandapower](https://img.shields.io/badge/pandapower-2.13.1-blue.svg)](https://pypi.org/project/pandapower/2.13.1/)
[![pandas](https://img.shields.io/badge/pandas-2.1.1-blue.svg)](https://pypi.org/project/pandas/2.1.1/)

This repository presents valid models of the Dutch high-voltage power system based on open data and open-source software. The models can run power flow and optimal power flow for a balanced AC power system. 

This work is published in a [conference paper](https://ieeexplore.ieee.org/document/9960703) titled: **"Open Data Based Model of the Dutch High-Voltage Power System"** by Wouter Zomerdijk, Digvijay Gusain, Peter Palensky, and Milos Cvetkovic.

## Basic grid models
The 

### Import and use
***Step 1:*** Install the following packages:  

[![pandapower](https://img.shields.io/badge/pandapower-2.13.1-blue.svg)](https://pypi.org/project/pandapower/2.13.1/)  
[![pandas](https://img.shields.io/badge/pandas-2.1.1-blue.svg)](https://pypi.org/project/pandas/2.1.1/)

***Step 2:*** Import one of the models:  
```bash
net = pandapower.from_pickle('grid_2021.p')
net = pandapower.from_pickle('aggregated_grid_2021.p')
```

### grid_2021.p
110/150/220/380 kV Dutch Power System in 2021. Includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower).

<img src="https://github.com/WZomerdijk/Dutch-HV-Power-System/assets/122889461/aa727cb4-dda8-497a-9280-7ece9b2b6df7" width="250">

### aggregated_grid_2021.p
220/380 kV Dutch Power System in 2021. Includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower).

<img src="https://github.com/WZomerdijk/Dutch-HV-Power-System/assets/122889461/84bc4cb4-692b-455f-9133-3fd849f18960" width="250">

## Simulation 
### aggregated_grid_2021_with_generators_costs.p
220/380 kV Dutch Power System in 2021. Includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower). Moreover, all generators are mapped to their corresponding buses and the operating costs are provided per generator type (for OPF).

### aggregated_grid_2018_with_generators_loads_costs_controllers.p
All sustainable generators (solar and wind) and loads have controllers that provide them with an hourly generation/consumption setpoint. For running an optimal power flow, the operating costs per generator type per hour in 2018 are also provided by the controller. These prices are based on the fuel prices, generator efficiencies, and the ETS prices for carbon emmissions.  

The last model was slightly adjusted to run a simulation for 2018. The simulation file is provided as *aggregated_grid_2018_with_generators_loads_costs_controllers.p*.  
This is the 220/380 kV Dutch Power System in 2018. It includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower). All generators are mapped to their corresponding buses and the operating costs are provided per generator type (for OPF). Furthermore, the aggregated loads are mapped to corresponding buses and controllers are implemented to run an hourly analysis on the entire Dutch power system for 2018.  
As this model is developed for analysis in 2018, generators that were not commissioned before 2018 were excluded.

The entire simulation can be ran by following these steps (provide an input for *time_steps*):  
```
pandapower.timeseries.run_timeseries(net, time_steps=[...], continue_on_divergence=True)
```



### Import and use
***Step 1:*** Install the following packages:  

[![pandapower](https://img.shields.io/badge/pandapower-2.13.1-blue.svg)](https://pypi.org/project/pandapower/2.13.1/)  
[![pandas](https://img.shields.io/badge/pandas-2.1.1-blue.svg)](https://pypi.org/project/pandas/2.1.1/)

***Step 2:*** Import one of the models:  
```bash
net = pandapower.from_pickle('aggregated_grid_2021_with_generators_costs.p')
net = pandapower.from_pickle('aggregated_grid_2018_with_generators_loads_costs_controllers.p')
```

***Step 3:*** Create output writer:  
In order to save the results of a timeseries simulation, you have to specificy the variables to save, and the file type. For example:
```bash
from pandapower.timeseries.output_writer import OutputWriter

ow = OutputWriter(net) # create an OutputWriter
ow.log_variable('res_bus', 'vm_pu') # add logging for bus voltage magnitudes
ow.log_variable('res_line', 'loading_percent') # add logging for line loadings in percent
```

***Step 4:*** Run a timeseries simulation:
```bash
pandapower.timeseries.run_time_series.run_timeseries(net, time_steps=None, continue_on_divergence=False, verbose=True, check_controllers=True, **kwargs)

pandapower.timeseries.run_timeseries(net, time_steps=[...], continue_on_divergence=True)
```

## Attribution
A paper describing the open data based model of the Dutch high-voltage power system was presented at IEEE ISGT Europe in 2022.  
If you want to use the model, please consider referencing the conference paper.
```
@inproceedings{DutchHVPowerSystem,
  author = {Zomerdijk, Wouter and Gusain, Digvijay and Palensky, Peter and Cvetkovic, Milos},
  title = {Open Data Based Model of the Dutch High-Voltage Power System}, 
  booktitle = {2022 IEEE Innov. Smart Grid Technol. Europe}, 
  address = {Novi Sad, Serbia},
  month = {Oct},
  year = {2022},
  pages = {1-6}}
```

## Extra information
The presented models are generated in pandapower and exported as pickle files.
The work underlying these models of the Dutch HV power system was performed in 2021 and the corresponding models have not been updated since then. For extra information on the underlying work and examples of analyses supported by these models, you can check the corresponding [MSc thesis](http://resolver.tudelft.nl/uuid:b9d69d1b-d2bf-4ce0-acc6-97171cde3568). For information on and questions related to pandapower datastructures/functions/etc. you can check the support page of [pandapower](https://pandapower.readthedocs.io/en/v2.13.1).  
