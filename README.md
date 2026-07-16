![TiMBA Logo](https://github.com/TI-Forest-Sector-Modelling/TiMBA/blob/main/timba_logo_v3.png?raw=true)  

-----------------

# TiMBA - Timber market Model for policy-Based Analysis



[![CI - Test](https://img.shields.io/github/actions/workflow/status/TI-Forest-Sector-Modelling/TiMBA/actions.yml?label=CI%20-%20Test)](https://github.com/TI-Forest-Sector-Modelling/TiMBA/actions/workflows/actions.yml)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=TI-Forest-Sector-Modelling_TiMBA&metric=coverage)](https://sonarcloud.io/summary/new_code?id=TI-Forest-Sector-Modelling_TiMBA)
![GitHub Release](https://img.shields.io/github/v/release/TI-Forest-Sector-Modelling/TiMBA)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.13842384.svg)](https://doi.org/10.5281/zenodo.13842384)
[![DOI](https://joss.theoj.org/papers/10.21105/joss.08034/status.svg)](https://doi.org/10.21105/joss.08034)
[![License](https://img.shields.io/github/license/TI-Forest-Sector-Modelling/TiMBA?style=flat)](https://github.com/TI-Forest-Sector-Modelling/TiMBA/blob/main/COPYING)

-----------------  

**TiMBA** is a partial economic equilibrium model for the global forest products market. The model endogenously simulates 
production, consumption, and trade of wood and wood-based products in 180 countries. TiMBA recursively computes the market 
equilibrium for each country and product in a given period by maximizing the social surplus in the global forest sector. 
In the equilibrium processes, product supply, demand and price are balanced for each simulation period. 

-----------------

- [TiMBA - Timber market Model for policy-Based Analysis](#timba---timber-market-model-for-policy-based-analysis)
  - [1. Cite TiMBA](#1-cite-timba)
  - [2. Installation](#2-installation)
    - [2.1. PyPI package](#21-pypi-package)
    - [2.2. GitHub repository](#22-github-repository)
  - [3. Use TiMBA](#3-use-timba)
  - [4. Model extensions](#4-model-extensions)
  - [5. Project structure](#5-project-structure)
  - [6. Model settings](#6-model-settings)
    - [6.1. Settings as parameters](#61-settings-as-parameters)
    - [6.2. Advanced settings](#62-advanced-settings)
  - [7. TiMBA extended model description](#7-timba-extended-model-description)
  - [8. Extensions for TiMBA](#8-extensions-for-timba)
  - [9. Roadmap and project status](#9-roadmap-and-project-status)
  - [10. Contributing to the project](#10-contributing-to-the-project)
  - [11. Authors](#11-authors)
  - [12. Contribution statement](#12-contribution-statement)
  - [13. License and copyright note](#13-license-and-copyright-note)
  - [14. Acknowledgements](#14-acknowledgements)
  - [15. References](#15-references)


-----------------

**TiMBA** is a partial economic equilibrium model for the global forest products
market. The model endogenously simulates production, consumption and trade of
wood and wood-based products in 180 countries. TiMBA recursively computes the
market equilibrium for each country and product in a given period by maximizing
the social surplus in the global forest sector. In the equilibrium processes,
product supply, demand and price are balanced for each simulation period.

## 1. Cite TiMBA

We are happy that you use TiMBA for your research. When publishing your work in articles, working paper, presentations 
or elsewhere, please cite the model as 

TI-FSM, Morland, C., Schier, F., Tandetzki, J., Honkomp, T. (2025). TiMBA (Timber market Model for policy-Based Analysis). Journal of Open Source Software, 10(115), 8034, [https://doi.org/10.21105/joss.08034](https://doi.org/10.21105/joss.08034)  
[Download BibTeX](./citation.bib)

The authors' collective is named Thünen Institute Forest Sector Modelling (TI-FSM). The individual authors are listed as 
co-authors in alphabetical order. 

## 2. Installation

The package is developed and tested with Python 3.9 on Windows 11. TiMBA is
compatible with Python versions between 3.9 - 3.11. The functionality with
supported Python versions is continuously tested using GitHub CI with [operating
system images](https://github.com/actions/runner-images#available-images)
`windows-latest` and `ubuntu-latest`.

### 2.1 Requirements

- Operating system: Linux or Windows on a x64 architecture. We test Windows 11
  and Ubuntu 24.04, but other Windows and Linux versions are likely to work too.
- The Python package manager [uv](https://docs.astral.sh/uv/). We strongly
  recommend to use `uv`, because it provides a very user-friendly, quick,
  platform-independent and reliable way to install, use and develop TiMBA.
  
That said, TiMBA can be also installed classically by creating a virtual
environment using [`venv`](https://docs.python.org/3.14/library/venv.html) and
installing with [`pip`](https://pip.pypa.io/en/stable/). To make the virtual
environment use the right Python version (if your system Python doesn't happen
to be version 3.9 - 3.11), you need to install a supported Python the the
operating system level and create the virtual environment with that version,
e.g. `python3.11 -m venv .venv`.

***Known Issues***:
TiMBA currently does not work with Python 3.12 or higher. We observe numerical discrepancies (>5% compared to results generated on Windows or Linux) when running TiMBA on macOS which could traced back to the solver OSQP in CVXPY. The results with MacOS have not been validated. We are investigating the issue.


### 2.1. PyPI package

```bash
pip install pytimba
````

Once installed, `timba` can be imported with standard settings:

```python
from timba.main import run_timba

run_timba()
```

To change the folder for input and output data, the user can use the `folderpath` option (note: the path must be a `Path` object from `pathlib`):

```python
from timba.main import run_timba
from pathlib import Path

run_timba(folderpath=Path(r"your_path"))
```

To modify specific parameters, the user can import `parameter_setter` from `TiMBA.main` and set new values:

```python
from timba.main import run_timba, parameter_setter

parameters = parameter_setter()
parameters.max_period = 2

run_timba(Parameters=parameters)
```

### 2.2. GitHub repository

1. Clone the repository
Begin by cloning the repository to your local machine using the following command: 
    >git clone https://github.com/TI-Forest-Sector-Modelling/TiMBA.git
   > 
2. Switch to the TiMBA directory  
Navigate into the TiMBA project folder on your local machine.
   >cd TiMBA
   >
3. Create a virtual environment  
It is recommended to set up a virtual environment for TiMBA to manage dependencies. The package is tested for 
   Python versions up to 3.11. With a newer Python version, we can not guarantee the full functionality of the package.
   Select the correct Python interpreter.   
   Show installed versions: 
   >py -0  
   >
   - If you have installed multiple versions of Python, activate the correct version using the py-Launcher.
   >py -3.11 -m venv venv 
   > 
   - If you are using only a single version of Python on your computer:
   >python -m venv venv
   >
4. Activate the virtual environment  
Enable the virtual environment to isolate TiMBA dependencies. 
   >venv\Scripts\activate

   Or for Linux:
      >source venv/bin/activate

5. Install TiMBA in the editable mode  
   >pip install -e .

    If the following error occurs: "ERROR: File "setup.py" or "setup.cfg" not found."
    you might need to update the pip version you use with: 
    >python.exe -m pip install --upgrade pip  
      

***Double check installation and test suite***:  
Double check if installation was successful by running following command from terminal:  
   >timba --help

The help provides you information about the basic model commands. 

The TiMBA model comes with a test suite to ensure its functionality.
Run the test suite to check the functionality of the package and validate the produced results with those provided by the
TI-FSM using the coverage report:

  > coverage run

To reduce the test suite running time, only the first period will be computed and compared. The test suite results will not be saved.
The computed results and provided validation results are compared with a relative tolerance of 5%.  

The coverage report of the TiMBA model can be accessed using:
 > coverage report


## 3. Use TiMBA

TiMBA provides a built-in command-line interface (CLI) that enables users to download input data, execute simulations, and analyse model outputs.

To run TiMBA, a scenario input file together with the required auxiliary datasets (e.g. country and commodity information) is required. The software package intentionally does **not** include any default datasets. Instead, software and data are maintained separately, allowing datasets to be versioned, cited, and updated independently.

The default input datasets are archived on Zenodo in the repository **[TiMBA Additional Information](https://doi.org/10.5281/zenodo.19466845)**. They can be downloaded directly via the CLI:

```bash
timba load
```

If no target directory is specified by the user, the datasets are downloaded into the current working directory. The command retrieves the latest public release of the TiMBA default scenario. Additional scenario datasets will be published separately alongside the corresponding scientific publications and then can be loaded separately.

The main entry point for executing simulations is:

```bash
timba run
```

If the required input data are not available locally, TiMBA automatically downloads the default dataset before starting the simulation. Consequently, an internet connection is required for the first execution.

TiMBA reads the scenario input file (`scenario_input.xlsx`) together with all required auxiliary data. A detailed description of the input data structure is provided in the section [TiMBA extended model description](#timba-extended-model-description-).

Although TiMBA can also be imported directly as a Python package, the CLI is the recommended interface for most users.

The available CLI commands are:

| Command     | Description                                                            |
|-------------|------------------------------------------------------------------------|
| `load`      | Download input datasets from the TiMBA data repository.                |
| `run`       | Execute TiMBA simulations.                                             |
| `carbon`    | Calculate forest carbon stocks in forests and harvested wood products. |
| `dashboard` | Launch an interactive dashboard for analysing simulation results.      |

By default, TiMBA uses the current working directory for input and output files. A different project directory can be specified using the `-FP` (folder path) option:

```bash
timba run -FP your_path
```

This allows users to organise multiple projects or customise model runs with alternative input datasets (see [Model settings](#model-settings) for further details).

To verify that the installation is working correctly, execute a simulation for the first model period only:

```bash
timba run -MP 1
```

## 4. Supplementary modules

The TiMBA ecosystem has a modular design. Each module extends the base functionality of TiMBA in a specific way.
TiMBA includes support for **[TiMBA Charts](https://doi.org/10.5281/zenodo.20925292)**, an interactive dashboard for exploring simulation results.

Launch the dashboard with:

```bash
timba dashboard
```

This opens a local web application in your browser, providing interactive visualisations of the most important model outputs.

By default, the dashboard loads the two most recent simulation results from the standard TiMBA output directory. If no simulation has been executed yet, the dashboard cannot be started. Users can also specify alternative output directories or select a different number of scenarios for comparison.

To launch the dashboard automatically after a simulation finishes, use:

```bash
timba run -SD True
```

TiMBA also supports a dedicated **[Carbon Module](https://zenodo.org/records/17814169)**:

```bash
timba carbon
```

This module estimates carbon stocks in forest biomass, forest soils, deadwood, litter, harvested wood products, and substitution effects based on TiMBA simulation results and historical statistics on forest resources and forest product markets ([Honkomp 2026](https://www.sciencedirect.com/science/article/pii/S2352711026000828#abs0001)).

## 5. Project structure

After downloading the input data (`timba load`), the project directory is organised as follows:

```text
data
└── input
    ├── 01_Input_Files
    │   └── scenario_input.xlsx          # Main scenario definition and model inputs
    ├── 02_Additional_Information
    │   ├── additional_information.xlsx  # Country, commodity and auxiliary information
    │   └── worldprice.xlsx              # Initial world price data
    └── 03_Serialization
        ├── AddInfoContent.pkl
        ├── WorldDataContent.pkl
        └── WorldPriceContent.pkl
```

The serialization files are generated automatically and contain cached representations of the processed input data. They are used to speed up subsequent model runs.

After simulation, TiMBA creates an `output` directory inside the `data` folder.

```text
data
└── output
    ├── TiMBA.log
    ├── DataContainer_<timestamp>.pkl
    ├── results_<timestamp>.csv
    ├── worldprices_<timestamp>.csv
    ├── forest_<timestamp>.csv
    ├── manufacture_<timestamp>.csv
    └── results_aggregated_<timestamp>.csv
```

The output files contain:

| File                                 | Description                                                   |
|--------------------------------------|---------------------------------------------------------------|
| `TiMBA.log`                          | Log file containing information about the simulation process. |
| `DataContainer_<timestamp>.pkl`      | Complete serialized simulation results for programmatic use.  |
| `results_<timestamp>.csv`            | Main simulation results in csv format.                        |
| `worldprices_<timestamp>.csv`        | World price results in csv format.                            |
| `forest_<timestamp>.csv`             | Forest resources-related results in csv format.               |
| `manufacture_<timestamp>.csv`        | Manufacturing sector-related results in csv format.           |
| `results_aggregated_<timestamp>.csv` | Results aggregated at the continental level in csv format.    |

To ensure reproducibility, TiMBA never overwrites existing simulation results.

Each model run creates a new set of output files using timestamp-based filenames, for example:

```text
results_D20260708T14-32-18.csv
```

This guarantees that previous simulation results remain available for comparison and documentation.


## 6. Model settings
Multiple settings are integrated for the ***timba run*** command to allow users to interact with the model and adapt the modelling parameters to their research interests.
The following chapter provides a brief overview of the model settings. A detailed description of the settings is provided in the model documentation (TI-FSM 2025). 

Basic model settings include:

| Option                              | Parameter                      | Type   | Default                              |
|-------------------------------------|--------------------------------|--------|--------------------------------------|
| `-Y`, `--year`                      | `year`                         | int    | default_year                         |
| `-MP`, `--max_period`               | `max_period`                   | int    | default_max_period                   |
| `-PP`, `--calc_product_price`       | `calc_product_price`           | str    | default_calc_product_price           |
| `-WP`, `--calc_world_price`         | `calc_world_price`             | str    | default_calc_world_price             |
| `-MB`, `--material_balance`         | `material_balance`             | str    | default_MB                           |
| `-GMB`, `--global_material_balance` | `global_material_balance`      | bool   | global_material_balance              |
| `-TF`, `--trans_imp_exp_factor`     | `transportation_impexp_factor` | float  | default_transportation_impexp_factor |
| `-S`, `--serialization`             | `serialization`                | bool   | serialization_flag                   |
| `-D`, `--dynamization`              | `dynamization_activated`       | bool   | dynamization_activated               |
| `-COQ`, `--cleaned_opt_quantity`    | `cleaned_opt_quantity`         | bool   | cleaned_opt_quantity                 |
| `-CP`, `--capped_prices`            | `capped_prices`                | bool   | capped_prices                        |
| `-VO`, `--verb_opt_log`             | `verbose_optimization_logger`  | bool   | verbose_optimization_logger          |
| `-VT`, `--verb_calc_log`            | `verbose_calculation_logger`   | bool   | verbose_calculation_logger           |
| `-FP`, `--folderpath`               | `folderpath`                   | Path   | `cwd`                                |
| `-C`, `--activate_cmodule`          | `activate_cmodule`             | bool   | False                                |


Basic add-on module settings include (see [add-on modules for TiMBA](#add-on-modules-for-timba)):
- The activation of the carbon module [default: True]

***Note:***  
TiMBA is delivered with a validated set of default settings that were tested for stability and consistency. These default parameters can be modified when executing the package via CLI or directly in `default_parameters.py`. Please note that any parameters specified in the CLI will overwrite those defined in `default_parameters.py`.  
Not all combinations of functionalities and settings have been tested or validated. In particular, shadow and calculated price modes for country- and product-level (PP) and world (WP) prices must be applied consistently. Mixing the two (e.g., PP="calculated_PP" and WP="shadow_WP") is currently not supported and may result in an error. 
  
### 6.1. Settings as parameters
The CLI provides access to basic model settings and their default values. 
Check if CLI command is registered and available on your computer by executing:

- >timba run --help

Default settings can be changed in the following way: (Note that the change of default settings as described below is for demonstration purposes only, and the results have not been validated.):
- > timba run -MP=5 -MB="RCG_specific" -CP="True"
 

For this example, TiMBA will simulate 5 periods using calculated prices as product prices and shadow prices as world market prices.

### 6.2. Advanced settings
In addition to the settings accessible via the CLI, users can control advanced settings through changes in `Defines.py` 
Advance settings include:
- solver settings (like accuracy, number of iterations and penalties)
- conversion factors

**Caution: The model results were validated for a selection of setting combinations.**   
Some setting combinations might not be coherent and can lead to errors in the simulations or to unreliable results. Those combinations have neither been tested nor validated.   
Note: TiMBA was tested with a pre-defined number of periods and respective periods' lenght. If the number of periods is changed, it is necessary to compare and adjust this in the ExogChange sheet in the input file.

## 7. TiMBA extended model description 
TiMBA is a partial economic equilibrium model for the global forest products market. The market equilibrium is subject to 
market clearance and constraints balancing necessary raw materials and produced wood products and limiting the trade (Samuelson 1952). 
The model structure distinguishes between raw, intermediate and end products. TiMBA differentiates three types of roundwood 
(wood fuel, coniferous and non-coniferous industrial roundwood), two additional raw products for paper production (other fibre pulp and waste paper), 
two intermediate products (mechanical and chemical pulp) and eight finished products (coniferous and non-coniferous sawnwoods, veneer sheets and plywood, 
particle board, fibreboard, newsprint, printing and writing paper, and other paper and paperboards). Except for sawnwoods, intermediate and end products are 
produced from a mix of coniferous and non-coniferous industrial roundwood. Scenario simulations with TiMBA are guided by parameters and assumptions shaping 
future developments. In the model framework, wood products are implicitly treated as perfect substitutes, regardless of their origin, as long as they belong 
to the same commodity group. As the optimization of the market equilibrium in a given year does not include an elasticity of substitution, demand is merely 
shifted by changes in income and price (Murray et al. 2004). The supply of roundwood depends on wood prices and forest development which in turn is basically 
determined by the growth dynamics of forest stock, the change in forest area, and harvest volumes. The GDP development indicating national incomes is an important driver of change. In TiMBA, demand for wood-based products is positively correlated to income, thus, an increase in income basically leads to an increase in demand. 
Forest area development and thus, timber supply is coupled to GDP per capita developments based on the concept of the environmental Kuznets curve (Panayotou 2004). 
In its basic version, TiMBA uses the assumptions made in the “Middle of the road” scenario described in “The Shared Socioeconomic Pathways” (the so called SSP2 scenario) to model future GDP developments and population growth. This scenario describes a world of modest population growth and where social, economic and technological trends continue similarly to historical patterns (Riahi et al. 2017). 
Price and income elasticities of demand are taken from Morland et al. (2018). Further exogenous specifications on technology 
developments (input-output coefficients and manufacturing cost) are estimated based historical developments from 1993-2020. Information on trade inertia are based on WTO data as provided in the GFPM (Buongiorno et al. 2015; GFPM version 1-29-2017-World500) while data on WTO Ad-valorem taxes rates were recalculated by the authors as described in Schier et al. (2026). 
The base year for the scenario simulations with the current version of TiMBA is 2020.
The input data used for simulation with TiMBA needs to be calibrated and provided in a source file prior to model runs. This file is provided together with the model (`scenario_input.xlsx`) as described in [3. Use TiMBA](#3-use-timba). 
The model input data calibration procedure is described in Buongiorno and Zhu (2015) and altered according to Schier et al. (2018). The input data for calibrating the model are obtained from three global databases: The FAO forestry statistics (FAOSTAT), the FAO Forest Global Resources Assessment (FAO 2020) and the World Bank Development Indicators (World Bank). 
The model output comprises information about production, consumption and trade quantities, and prices as well as forest development. 
The model concept bases on the formal description of the Global Forest Products Model (GFPM) (Buongiorno et al. 2015, Buongiorno et al. 2003). 

## 8. Supplementary modules for TiMBA
TiMBA is designed as a modular modelling framework that can be extended with additional applications to enable further functionalities. 
Depending on the use case and the research question, users can activate or deactivate these modular extensions via the CLI or
through the `default_parameters`. In this way, the computational load is tailored to the user's needs. The modules are designed
as separate packages which can be imported into the main TiMBA application. These packages are usually hosted on the
TI-FSM [GitHub](https://github.com/orgs/TI-Forest-Sector-Modelling/repositories) or [PyPI](https://pypi.org/user/TI-FSM/)
pages. While full compatibility of extensions with each other is targeted, certain combinations of functionalities may
cause errors or lead to longer calculation times. Such issues can be reported in the respective repositories.

This sections provides an overview of available extensions. Beyond the activation, the extensions offer a range of
settings to adapt their functionality to specific use cases. For details about each module and its configuration options,
users should refer the respective GitHub repositories.   

|    Module     |                                                                                                                                                         Description                                                                                                                                                         |                                                                         Activation                                                                          |                               GitHub project                               |                                           Citation                                           |
|:-------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------:|
| Carbon Module | tracks carbon stocks and stock changes across pools in the forestry sector including aboveground and belowground biomass, forest soils, deadwood and litter, and harvested wood products. The module applies updated guidelines of the IPCC (2019). A visualization dashbord for carbon results is generated automatically. |                                    `activate_cmodule=True` in `default_parameters.py` <br/>or<br/> `-C=True` in the CLI                                     |     [C-Module](https://github.com/TI-Forest-Sector-Modelling/C-Module)     |                    [Honkomp (2025)](https://zenodo.org/records/16912178)                     |
| TiMBA Charts  |                                                                                                                          an analysis toolbox with multiple dashboards about the main TiMBA results                                                                                                                          | `timba run -SD True` to open the toolbox directly after simulation has ended, or `timba dashboard` to open the toolbox indepently from a specific TiMBA run | [TiMBA Charts](https://github.com/TI-Forest-Sector-Modelling/TiMBA_Charts) | [Morland, C., Tandetzki, J., & Honkomp, T. (2026)](https://doi.org/10.5281/zenodo.20925292)  |

## 9. Roadmap and project status

The development of TiMBA is ongoing and we are already working on future releases.

- To provide an easy way to visualize and analyse the output of TiMBA we published the interactive analysis toolbox TiMBA_Charts (Morland et al. 2025) that is also available in our [TiMBA repository](https://github.com/TI-Forest-Sector-Modelling/TiMBA). 

Several projects are currently extending different components of TiMBA:
- In the project [iNFORSu](https://www.thuenen.de/en/institutes/forestry/projects-1/modelling-of-the-global-roundwood-supply), we are working on the revision of the module computing forest area and stock development. Forest area development should not longer be only depend on the of GDPpc. Instead, further drivers significantly shaping forest area could be included into the simulation. In addition, the authors are working on the utilization of Global Vegetation Models (GVM) results on forest area development and net-primary production for integration into TiMBA simulations. This should bring forest development and thus, wood supply closer to reality. 
- The high flexibility of TiMBA allows it to cover a large panel of policy designs and conduct sensitivity analyses. In the project [BioSDG](https://www.thuenen.de/en/institutes/forestry/projects-1/the-bioeconomy-and-the-sustainable-development-goals-of-the-united-nations-biosdg) and [CarbonLeak](https://www.thuenen.de/en/cross-institutional-projects/carbon-leak), we extended the model to track and quantify carbon fluxes and stocks related to the forest sectors. Based on IPCC-based methods, carbon stocks in forest biomass and harvested wood products as well as substitution effects are quantified for each simulation period. This extension enables in-depth impact assessments of forest-based climate mitigation policies (e.g., carbon pricing policies and mitigation target-setting policies for the Land Use, Land Use Change and Forestry sector). 
- In another project, we are implementing bilateral trade flows into the model framework. This step is important to enhance policy impact assessments on e.g., leakage effects.
- Given the fast processing time, we are extending TiMBA to conduct exhaustive uncertainty analysis using Monte Carlo simulations. While these Monte Carlo simulations are currently used in the quantification of carbon stocks, their applications can be deployed to any input data of the model.


Frequently check [TiMBA repository](https://github.com/TI-Forest-Sector-Modelling/TiMBA) for new releases.

## 10. Contributing to the project
We welcome contributions, additions, and suggestion to further develop or improve the code and the model. To check, discuss and include them into this project, we would like you to share your ideas with us so that we can agree on the requirements needed for accepting your contribution. 
You can contact us directly via GitHub by creating issues, or by writing an Email to:

[wf-timba@thuenen.de](mailto:wf-timba@thuenen.de)

So far, this README serves as a comprehensive introduction and guidance on how to get started. ´The model documentation (TI-FSM 2025) and model validation (TI-FSM 2026) enables a deeper dive.



## 11. Authors
TiMBA was developed and written by an authors' collective named Thünen Institute Forest Sector Modelling (TI-FSM). 

The individual authors are listed in alphabetical order 
- [Christian Morland](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/ehemalige-liste/christian-morland-msc) [(ORCID 0000-0001-6600-570X)](https://orcid.org/0000-0001-6600-570X), 
- [Franziska Schier](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/dipl-ing-franziska-schier) [(ORCID 0000-0002-3378-1371)](https://orcid.org/0000-0002-3378-1371), 
- [Julia Tandetzki](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/julia-tandetzki-msc) [(ORCID 0000-0002-0630-9434)](https://orcid.org/0000-0002-0630-9434), and 
- [Tomke Honkomp](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/tomke-honkomp-msc) [(ORCID 0000-0002-6719-0190)](https://orcid.org/0000-0002-6719-0190). 

## 12. Contribution statement
Within the authors' collective TI-FSM, the authors have contributed over years their individual strengths and knowledge to make the model work:

| Author            | Conceptualization and theoretical framework | Methodology | Data Curation and Management | Formal Analysis | Programming | Writing and Documentation | Visualization | Review and Editing | Supervision |
|:------------------|:-------------------------------------------:|:-----------:|:----------------------------:|:---------------:|:-----------:|:-------------------------:|:-------------:|:------------------:|:-----------:|
| Christian Morland |                      X                      |      X      |              X               |        X        |      X      |             X             |       X       |         X          |             |
| Franziska Schier  |                      X                      |      X      |              X               |        X        |             |             X             |               |         X          |      X      |
| Julia Tandetzki   |                      X                      |      X      |              X               |        X        |      X      |             X             |       X       |         X          |             |
| Tomke Honkomp     |                      X                      |      X      |              X               |        X        |      X      |             X             |       X       |         X          |             |

## 13. License and copyright note

Licensed under the GNU AGPL, Version 3.0. 

Copyright ©, 2024, Thuenen Institute, TI-FSM, wf-timba@thuenen.de

 This program is free software: you can redistribute it and/or modify
 it under the terms of the GNU Affero General Public License as
 published by the Free Software Foundation, either version 3 of the
 License, or (at your option) any later version.

 This program is distributed in the hope that it will be useful, but
 WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
 Affero General Public License for more details.

 You should have received a copy of the GNU Affero General Public
 License along with this program.  If not, see
 <https://www.gnu.org/licenses/agpl-3.0.txt>.



## 14. Acknowledgements

This work is the result of great joint efforts of the forest products market analysis team at the Thünen Institute of Forestry and others from 2018 to 2024. In the last years, many people made important contributions to this work. Without their support, reflection, and constructive criticism, this undertaking would not have been as successful as it turns out to be now. We would like express our gratitude to all of them. In particular, we would like to thank 
-	Pixida GmbH and especially Tobias Hierlmeier for professional support in revising and restructuring the model architecture and code and being valuable help in programming tasks
-	Thünen Institute Service Centre for Research Data Management and especially Harald von Waldow for providing expertise, consultation, and support during the release process
-	Holger Weimar and Matthias Dieter for the trustful and cooperative working environment, valuable support and critical discussion and the opportunity to keep on going
-	Johanna Schliemann and Gregor Müller for technical support whenever needed
-	The Thünen Institut of Forestry and its Head Matthias Dieter for providing financial resources over the years 
- [makeareadme.com](https://www.makeareadme.com/) for providing the template this README is leaned on.

## 15. References
- Buongiorno, J.; Zhu, S.; Zhang, D.; Turner, J.; Tomberlin, D. The Global Forest Products Model; Academic Press: Cambridge, MA, USA, 2003; ISBN 978-0-12-141362-0
- Buongiorno, J. Global modelling to predict timber production and prices: The GFPM approach. Forestry 2015, 88, 291–303.
- Buongiorno, J.; and Zhu, S. 2015. Technical change in forest sector models: The GFPM approach.  Scand. J. For. Research, 30, 30-48.
- GFPM - Global Forest Product Model is available at https://onedrive.live.com/?authkey=%21AEF7RY7oAPlrDPk&id=93BC28B749A1DFB6%21118&cid=93BC28B749A1DFB6
- FAO. Global Forest Resources Assessment: Terms and Definitions; Forest Resources Assessment Working Paper 188; FAO: Rome, Italia, 2020; Available online: http://www.fao.org/3/I8661EN/i8661en.pdf
- FAO. Global Forest Resources Assessment. 2022. Available online: https://fra-data.fao.org/
- FAOSTAT. Forestry Production and Trade: Datenbank. Available online: https://www.fao.org/faostat/en/#data/FO
- Morland, C.; Schier, F.; Janzen, N.; Weimar, H. Supply and demand functions for global wood markets: Specification and plausibility testing of econometric models within the global forest sector. For. Policy Econ. 2018, 92, 92–105
- Morland, C.; Tandetzki, J.; & Honkomp, T. (2025). TiMBA Charts (v0.2.0). Zenodo. https://doi.org/10.5281/zenodo.15689299
- Murray, B.C.; McCarl, B.A.; Lee, H.-C. Estimating Leakage from Forest Carbon Sequestration Programs. Land Econ. 2004, 80, 109–124
- Panayotou, T. Empirical Tests and Policy Analysis of Environmental Degradation at Different Stages of Economic Development; Working Paper No. 238; International Labour Organization: Geneva, Switzerland, 1993; Available online: http://www.ilo.org/public/libdoc/ilo/1993/93B09_31_engl.pdf 
- Riahi, K.; van Vuuren, D.P.; Kriegler, E.; Edmonds, J.; O’Neill, B.C.; Fujimori, S.; Bauer, N.; Calvin, K.; Dellink, R.; Fricko, O.; et al. The Shared Socioeconomic Pathways and their energy, land use, and greenhouse gas emissions implications: An overview. Glob. Environ. Chang. 2017, 42, 153–168.
- Samuelson, Paul A. Spatial Price Equilibrium and Linear Programming; The American Economic Review, 1952, 42 (3), 283–303; Available online http://www.jstor.org/stable/1810381.
- Schier, F.; Morland, C.; Tandetzki, J.; Honkomp, T. (2026). TI-Forest-Sector-Modelling/TiMBA_Additional_Information: Reworking data files and licenses (Version v1.0.2). Zenodo. https://doi.org/10.5281/zenodo.19466845
- TI-FSM (2025) TiMBA - Timber market Model for policy-Based Analysis: Documentation of model structure, data, and parameters. Braunschweig: Johann Heinrich von Thünen-Institut, 35 p, Thünen Working Paper 263, DOI:10.3220/253-2025-16
- TI-FSM (2026) TiMBA - Timber market Model for policy-Based Analysis: Validation of a partial equilibrium model. Braunschweig: Johann Heinrich von Thünen-Institut, 36 p, Thünen Working Paper 282, DOI:10.3220/253-2026-29
- World Bank. World Development Indicators|DataBank. Available online: https://databank.worldbank.org/source/world-development-indicators
