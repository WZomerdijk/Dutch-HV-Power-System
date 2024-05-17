# Open Data Based Model of the Dutch High-Voltage Power System

![Static Badge](https://img.shields.io/badge/MADE_WITH-PYTHON_%26_PANDAPOWER-orange?style=for-the-badge)

[![python](https://img.shields.io/badge/python-3.10.13-blue.svg)](https://www.python.org/)
[![pandapower](https://img.shields.io/badge/pandapower-2.13.1-blue.svg)](https://pypi.org/project/pandapower/2.13.1/)
[![pandas](https://img.shields.io/badge/pandas-2.1.1-blue.svg)](https://pypi.org/project/pandas/2.1.1/)

This repository presents valid models of the Dutch high-voltage power system based on open data and open-source software. The models can run power flow and optimal power flow for a balanced AC power system. 

This work is published in a [conference paper](https://ieeexplore.ieee.org/document/9960703) titled: **"Open Data Based Model of the Dutch High-Voltage Power System"** by Wouter Zomerdijk, Digvijay Gusain, Peter Palensky, and Milos Cvetkovic.

## Description of basic grid models
* **_grid_2021.p:_** Includes all 110/150/220/380 kV components of the Dutch power system in 2021. It includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower).
* **_aggregated_grid_2021.p:_** Includes all components of the 220/380 kV part of the Dutch power system in 2021. It includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower).
* **_aggregated_grid_2021_with_generators_costs.p:_** Includes the same 220/380 kV components as the _aggregated_grid_2021.p_ model. It further includes all generators, which are mapped to their corresponding connecting buses, and the average operating costs per generator type (for OPF). These costs are based on the fuel prices, generator efficiencies, and the ETS prices for carbon emmissions in 2021.

<div align="center">
  <img src="https://github.com/WZomerdijk/Dutch-HV-Power-System/assets/122889461/d6a7e107-c43c-4351-a452-1b0e36c452e7" width="275" height="300">
  <img src="https://github.com/WZomerdijk/Dutch-HV-Power-System/assets/122889461/4f6dbe9d-ebc5-48d3-aec4-07462496769b" width="255" height="300">
</div>

### Installation

***Step 1:*** Install the following packages:  
```bash
pip install pandapower==2.13.1 pandas==2.1.1
```

***Step 2:*** Import one of the models:  
```python
net = pandapower.from_pickle('grid_2021.p')
# or
net = pandapower.from_pickle('aggregated_grid_2021.p')
# or
net = pandapower.from_pickle('aggregated_grid_2021_with_generators_costs.p')
```

***Optional - Step 3:*** Add solar and wind profiles to the static generators:  
In the specific case of _aggregated_grid_2021_with_generators_costs.p_, you need to add solar and wind profiles to the sustainable generators before adding loads and running the OPF. As the sustainable generators are modelled as static generators, due to their inherent characteristics, and inflexible and weather-dependent nature, you need to multiply their maximum capacity by the timeseries of normalized yearly production.
```python
from pandapower.timeseries.data_sources.frame_data import DFData

normalized_profile_PV = ... # add profile of normalized yearly solar production
normalized_profile_WP = ... # add profile of normalized yearly wind production

for i in range(len(net.sgen)):
  if net.sgen.type.loc[i] == 'PV': # add controller for only the solar elements
    pandapower.control.ConstControl(net, element='sgen', variable='max_p_mw', element_index=i, data_source=DFData(net.sgen.p_mw[i] * normalized_profile_PV))
  elif net.sgen.type.loc[i] == 'WP': # add controller for only the wind elements
    pandapower.control.ConstControl(net, element='sgen', variable='max_p_mw', element_index=i, data_source=DFData(net.sgen.p_mw[i] * normalized_profile_WP))
```

## Description of validation model
- **_aggregated_grid_2018_with_generators_loads_costs_controllers.p:_** Includes the 220/380 kV components of the Dutch power system in 2018. It includes HV buses, HV lines, HV/MV transformers, and connections to other countries (as external grids in pandapower). All generators are mapped to their corresponding buses and the operating costs are provided per generator type (for OPF). These costs are based on the fuel prices, generator efficiencies, and the ETS prices for carbon emmissions in 2018. Furthermore, the aggregated loads are mapped to corresponding buses and controllers are implemented to run an hourly analysis on the entire Dutch power system for 2018. As this model is developed for analysis in 2018, generators that were not commissioned before 2018 were excluded.

### Installation

***Step 1:*** Install the following packages:  
```bash
pip install pandapower==2.13.1 pandas==2.1.1
```

***Step 2:*** Import the model:  
```python
net = pandapower.from_pickle('aggregated_grid_2018_with_generators_loads_costs_controllers.p')
```

***Step 3:*** Create output writer:  
In order to save the results of a timeseries simulation, you have to specificy the variables to save, and the file type. For example:
```python
from pandapower.timeseries.output_writer import OutputWriter

ow = OutputWriter(net) # create an OutputWriter
ow.log_variable('res_bus', 'vm_pu') # add logging for bus voltage magnitudes
ow.log_variable('res_line', 'loading_percent') # add logging for line loadings in percent
```

***Step 4:*** Run a timeseries simulation:
```python
n_ts = 8760 # number of hours in one year
ts = range(0, n_ts)
pandapower.timeseries.run_timeseries(net, time_steps=[ts], continue_on_divergence=True)
```

***Optional:***  
Check timeseries data for any of the load, costs, or sustainable generator controllers:
All sustainable generators (solar and wind) and loads have controllers that provide them with an hourly generation/consumption setpoint. For running an optimal power flow, the operating costs per generator type per hour in 2018 are also provided by the controller. 
```python
net.controller.object[...].data_source.to_dict()
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
