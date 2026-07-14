![TiMBA Logo](https://github.com/TI-Forest-Sector-Modelling/TiMBA/blob/main/timba_logo_v3.png?raw=true)  

-----------------

# TiMBA - Timber market Model for policy-Based Analysis

[![CI - Test](https://img.shields.io/github/actions/workflow/status/TI-Forest-Sector-Modelling/TiMBA/actions.yml?label=CI%20-%20Test)](https://github.com/TI-Forest-Sector-Modelling/TiMBA/actions/workflows/actions.yml)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=TI-Forest-Sector-Modelling_TiMBA&metric=coverage)](https://sonarcloud.io/summary/new_code?id=TI-Forest-Sector-Modelling_TiMBA)
![GitHub Release](https://img.shields.io/github/v/release/TI-Forest-Sector-Modelling/TiMBA)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.13842384.svg)](https://doi.org/10.5281/zenodo.13842384)
[![DOI](https://joss.theoj.org/papers/10.21105/joss.08034/status.svg)](https://doi.org/10.21105/joss.08034)
[![License](https://img.shields.io/github/license/TI-Forest-Sector-Modelling/TiMBA)](https://github.com/TI-Forest-Sector-Modelling/TiMBA/blob/main/COPYING)

-----------------

<!-- TOC -->
- [Cite TiMBA](#cite-timba)
- [Install TiMBA](#install-timba)
    - [Quick start with uv](#quick-start-with-uv)
    - [Known Issues](#known-issues)
    - [Installation Process](#installation-process)
    - [Double check installation and test suite](#double-check-installation-and-test-suite)
- [Use TiMBA](#use-timba)
    - [Model settings](#model-settings)
      - [Settings as parameters](#settings-as-parameters)
      - [Advanced settings](#advanced-settings)
- [TiMBA extended model description](#timba-extended-model-description)
- [Extensions for TiMBA](#extensions-for-timba)
- [Roadmap and project status](#roadmap-and-project-status)
- [Contributing to the project](#contributing-to-the-project)
- [Authors](#authors)
- [Contribution statement](#contribution-statement)
- [License and copyright note](#license-and-copyright-note)
- [Acknowledgements](#acknowledgements)
- [References](#references)

<!-- /TOC -->

**TiMBA** is a partial economic equilibrium model for the global forest products
market. The model endogenously simulates production, consumption and trade of
wood and wood-based products in 180 countries. TiMBA recursively computes the
market equilibrium for each country and product in a given period by maximizing
the social surplus in the global forest sector. In the equilibrium processes,
product supply, demand and price are balanced for each simulation period.

## Cite TiMBA

We are happy that you use TiMBA for your research. When publishing your work in articles, working paper, presentations or elsewhere, please cite the model as 

TI-FSM, Morland, C., Schier, F., Tandetzki, J., Honkomp, T. (2025). TiMBA (Timber market Model for policy-Based Analysis). Journal of Open Source Software, 10(115), 8034, [https://doi.org/10.21105/joss.08034](https://doi.org/10.21105/joss.08034)  
[Download BibTeX](./citation.bib)

The authors' collective is named "Thünen Institute Forest Sector Modelling
(TI-FSM)"". The individual authors are listed as Co-authors in alphabetical
order.

## Install TiMBA

The package is developed and tested with Python 3.9 on Windows 11. TiMBA is
compatible with Python versions between 3.9 - 3.11. The functionality with
supported Python versions is continuously tested using GitHub CI with [operating
system images](https://github.com/actions/runner-images#available-images)
`windows-latest` and `ubuntu-latest`.

### Requirements

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

### Install uv

- **Linux**:
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```
- **Windows**:
  ```powershell
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Install the TiMBA command line programs

If you want to use TiMBA as executable program, install it globally like so:

``` bash
uv tool install -p 3.11 pytimba
```
You will then have access to the CLI commands `load_timba`, `run_carbon_module`, `run_timba`. Usage instructions are available with the argument `--help` for each of the commands. 
See section [Model settings](#model-settings) for further details.

### Install TiMBA from PyPi

If you want to use TiMBA as part of your own program, you can install it from PyPi like so:

``` bash
uv init -p 3.11 my_timba_program  # Create a project skeleton with Python 3.11
cd my_timba_program               # Change into the project directory
uv add pytimba                    # Install TiMBA into the project's virtual environment.
```
Use TiMBA in Python modules, for example

```python
from TiMBA.main import run_timba

run_timba()
```

To change the folder for input and output data, the user can use the `folderpath` option (note: the path must be a `Path` object from `pathlib`):

```python
from TiMBA.main import run_timba
from pathlib import Path

run_timba(folderpath=Path(r"your_path"))
```

To modify specific parameters, you can import `parameter_setter` from `TiMBA.main` and set new values:

```python
from TiMBA.main import run_timba, parameter_setter

parameters = parameter_setter()
parameters.max_period = 2

run_timba(Parameters=parameters)
```

### Install TiMBA from GitHub

To install TiMBA in editable mode, so that you can modify the TiMBA source code, install it directly from GitHub: 

1. Clone the repository and move into the project folder:
   ```bash
   git clone https://github.com/TI-Forest-Sector-Modelling/TiMBA.git timba
   cd timba
   ```
2. Create the virtual environment and install all dependencies into it:
   ```bash
   uv sync
   ```
3. Verify the installation and run a first period:
   ```bash
   uv run run_timba --help
   uv run run_timba -MP=1
   ```

### Testing TiMBA

The TiMBA model comes with a test suite to ensure its functionality. Run the
test suite to check the functionality of the package and validate the produced
results with those provided by the TI-FSM using the coverage report:

   ```bash
   uv run python -W ignore::DeprecationWarning -m coverage run --rcfile=.coveragerc -m unittest discover
   uv run coverage report
   ```
   
To reduce the test suite running time, only the first period will be computed
and compared. The test suite results will not be saved. The computed results and
provided validation results are compared with a relative tolerance of 5%.

### Known Issues
Python versions >=3.12 are currently not supported because the pinned
dependencies (e.g. pandas) are not compatible with those Python versions. We
recommend using Python 3.9–3.11 for best results.


## Use TiMBA
The package comes with a built-in CLI to compute the TiMBA for various inputs. While arguments are documented in the output of calling `run_timba --help` from the terminal, an important part to mention is user input data that need to be imported from a selected folder. You shall not change the following structure within the data folder:  
TiMBA is provided with an input file (scenario_input.xlsx), including all input data necessary to run the model. The section [TiMBA extended model description](#timba-extended-model-description-) provides a detailed description of the included input data.
```bash
.
`- data
  `-- input
    `-- 01_Input_Files
      |-- scenario_input.xlsx #contains all input data to the model. 
    `-- 02_Additional_Information
      |-- additional_information.xlsx 
      |-- worldprice.xlsx
    `-- 03_Serialization
      |-- AddInfoContent.pkl #contains information about the last input data which is processed by the model
      |-- WorldDataContent.pkl #contains information about the last input data which is processed by the model
      |-- WorldPriceContent.pkl #contains information about the last input data which is processed by the model
```

The package will generate a result directory called `output` which is located inside the data folder. The final directory after one run will look something like this:
```bash
.
`- data
  `-- output
      |-- ....log #contains the logged process of the simulation
      |-- DataContainer_....pkl #contains all output information as pkl file
      |-- results....csv #contains main results as csv file
      |-- worldprices....csv #contains world price results as csv file
      |-- forest....csv #contains forest area and stock results as csv file
      |-- manufacture....csv #contains results for manufacturing as csv 
      |-- results_aggregated....csv #contains aggregated results on continent level as csv file

```
**Important output information**  
No output file will ever be overwritten by the application itself. New
results-files will be generated in the format
`results_D<yyyymmdd>T<hh-mm-ss>.csv` and will be saved to the output folder as
well. The logfile itself won't be overwritten as well but also no new file
created on additional runs. Log information simply gets appended to the existing
logfile. Removing the logfile ahead of executing the model won't result in
errors.

### Model settings

Multiple settings are integrated in TiMBA to allow users to interact with the
model and adapt the modelling parameters to their research interests. The following
chapter provides a brief overview of the model settings. A detailed description
of the settings is provided in the documentation.

Basic model settings include:
| Option                              | Parameter                      | Type  | Default                              |
| ----------------------------------- | ------------------------------ | ----- | ------------------------------------ |
| `-Y`, `--year`                      | `year`                         | int   | default_year                         |
| `-MP`, `--max_period`               | `max_period`                   | int   | default_max_period                   |
| `-PP`, `--calc_product_price`       | `calc_product_price`           | str   | default_calc_product_price           |
| `-WP`, `--calc_world_price`         | `calc_world_price`             | str   | default_calc_world_price             |
| `-MB`, `--material_balance`         | `material_balance`             | str   | default_MB                           |
| `-GMB`, `--global_material_balance` | `global_material_balance`      | bool  | global_material_balance              |
| `-TF`, `--trans_imp_exp_factor`     | `transportation_impexp_factor` | float | default_transportation_impexp_factor |
| `-S`, `--serialization`             | `serialization`                | bool  | serialization_flag                   |
| `-D`, `--dynamization`              | `dynamization_activated`       | bool  | dynamization_activated               |
| `-COQ`, `--cleaned_opt_quantity`    | `cleaned_opt_quantity`         | bool  | cleaned_opt_quantity                 |
| `-CP`, `--capped_prices`            | `capped_prices`                | bool  | capped_prices                        |
| `-VO`, `--verb_opt_log`             | `verbose_optimization_logger`  | bool  | verbose_optimization_logger          |
| `-VT`, `--verb_calc_log`            | `verbose_calculation_logger`   | bool  | verbose_calculation_logger           |
| `-FP`, `--folderpath`               | `folderpath`                   | Path  | `cwd`                                |
| `-C`, `--activate_cmodule`          | `activate_cmodule`             | bool  | False                                |


Basic add-on module settings include (see [add-on modules for TiMBA](#add-on-modules-for-timba)):
- The activation of the carbon module [default: True]

***Note:***  
TiMBA is delivered with a validated set of default settings that were tested for stability and consistency. These default parameters can be modified when executing the package via CLI or directly in `default_parameters.py`. Please note that any parameters specified in the CLI will overwrite those defined in `default_parameters.py`.  
Not all combinations of functionalities and settings have been tested or validated. In particular, shadow and calculated price modes for country- and product-level (PP) and world (WP) prices must be applied consistently. Mixing the two (e.g., PP="calculated_PP" and WP="shadow_WP") is currently not supported and may result in an error. 
  
#### Settings as parameters
The CLI provides to access basic model settings, and their default values. 
Check if CLI command is registered and available on your computer by executing either:

- >timba_run --help

Default settings can be changed in the following way: (Note: The change of default settings as described below is for demonstration purposes only, and the results have not been validated.):
- > run_timba -MP=5 -MB="RCG_specific" -CP="True"
 
For this example, TiMBA will simulate 5 periods using calculated prices as product prices and shadow prices as world market prices.

#### Advanced settings
In addition to the settings accessible via the CLI, users can control advanced settings through changes in `Defines.py` 
Advance settings include:
- solver settings (like accuracy, number of iterations and penalties)
- conversion factors

**Caution: The model results were validated for a selection of setting combinations.**   
Some setting combinations might not be coherent and can lead to errors in the simulations or to unreliable results. Those combinations have neither been tested nor validated.   
Note: TiMBA has been tested for periods one to nine. Additionally, if the number of periods is changed, it is necessary to compare and adjust this in the ExogChange sheet in the input file.

## TiMBA extended model description 
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
determined by the growth dynamics of forest stock, the change in forest area, and harvest volumes. The GDP development indicating national incomes is an important 
driver of change. In TiMBA, demand for wood-based products is positively correlated to income, thus, an increase in income basically leads to an increase in demand. 
Forest area development and thus, timber supply is coupled to GDP per capita developments based on the concept of the environmental Kuznets curve (Panayotou 2004). 
In its basic version, TiMBA uses the assumptions made in the “Middle of the road” scenario described in “The Shared Socioeconomic Pathways” (the so called SSP2 scenario) to 
model future GDP developments and population growth. This scenario describes a world of modest population growth and where social, economic and technological trends continue 
similarly to historical patterns (Riahi et al. 2017). Price and income elasticities of demand are taken from Morland et al. (2018). Further exogenous specifications on technology 
developments (input-output coefficients and manufacturing cost) are estimated based historical developments from 1993-2020. Information on trade inertia are based on 
WTO data as provided in the GFPM (Buongiorno et al. 2015; GFPM version 1-29-2017-World500) while data on WTO Ad-valorem taxes rates are taken from Schier et al. (2025). 
The base year for the scenario simulations with the current version of TiMBA is 2020.
The input data used for simulation with TiMBA needs to be calibrated and provided in a source file prior to model runs. This file is provided together with the model. 
The calibration procedure is described in Buongiorno and Zhu (2015) and altered according to Schier et al. (2018). The input data for calibrating the model are 
obtained from three global databases: The FAO forestry statistics (FAOSTAT), the FAO Forest Global Resources Assessment (FAO 2020) and the World Bank Development 
Indicators (World Bank). The model output comprises information about production, consumption and trade quantities, and prices as well as forest development. The 
model concept bases on the formal description of the Global Forest Products Model (GFPM) (Buongiorno et al. 2015, Buongiorno et al. 2003). 

## Extensions for TiMBA
TiMBA is a flexible modelling framework that can be extended with additional applications to enable further functionalities. 
Depending on the use case and the research question, users can activate or deactivate these modular extensions via the CLI or
through the `default_parameters`. In this way, the computational load is tailored to the user's needs. The modules are designed
as separate packages which can be imported into the main TiMBA application. These packages are usually hosted on the
TI-FSM [GitHub](https://github.com/orgs/TI-Forest-Sector-Modelling/repositories) or [PyPI](https://pypi.org/user/TI-FSM/)
pages. While full compatibility of extensions with each other is targeted, certain combinations of functionalities may
cause errors or lead to longer calculation times. Such issues can be reported in the respective repositories.

This sections provides an overview of available extensions. Beyond the activation, the extensions offer a range of
settings to adapt their functionality to specific use cases. For details about each module and its configuration options,
users should refer the respective GitHub repositories.   

|    Module     |                                                                                                                                                         Description                                                                                                                                                         |                                      Activation                                      |                           GitHub project                            |                       Citation                        |
|:-------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:|:-------------------------------------------------------------------:|:-----------------------------------------------------:|
| Carbon Module | tracks carbon stocks and stock changes across pools in the forestry sector including aboveground and belowground biomass, forest soils, deadwood and litter, and harvested wood products. The module applies updated guidelines of the IPCC (2019). A visualization dashbord for carbon results is generated automatically. | `activate_cmodule=True` in `default_parameters.py` <br/>or<br/> `-C=True` in the CLI | [C-Module](https://github.com/TI-Forest-Sector-Modelling/C-Module)  | [Honkomp (2025)](https://zenodo.org/records/16912178) |



## Roadmap and project status

The development of TiMBA is ongoing and we are already working on future releases.

- To provide an easy way to visualize and analyse the output of TiMBA we published the interactive analysis toolbox TiMBA:_charts (Morland et al. 2025) that is also available in our [TiMBA repository](https://github.com/TI-Forest-Sector-Modelling/TiMBA). 

Several projects are currently extending different components of TiMBA:
- In the project [iNFORSu](https://www.thuenen.de/en/institutes/forestry/projects-1/modelling-of-the-global-roundwood-supply), we are working on the revision of the module computing forest area and stock development. Forest area development should not longer be only depend on the of GDPpc. Instead, further drivers significantly shaping forest area shall be included into the simulation. This should bring forest development and thus, wood supply closer to reality. 
- In the project [BioSDG](https://www.thuenen.de/en/institutes/forestry/projects-1/the-bioeconomy-and-the-sustainable-development-goals-of-the-united-nations-biosdg) and [CarbonLeak](https://www.thuenen.de/en/cross-institutional-projects/carbon-leak), we extended the model to track and quantify carbon fluxes and stocks related to the forest sectors. Based on IPCC-based methods, carbon stocks in forest biomass and harvested wood products as well as substitution effects are quantified for each simulation period. This extension enables in-depth impact assessments of forest-based climate mitigation policies (e.g., carbon pricing policies and mitigation target-setting policies for the Land Use, Land Use Change and Forestry sector). The high flexibility of TiMBA allows it to cover a large panel of policy designs and conduct sensitivity analyses.   
- In another project, we are implementing bilateral trade flows into the model framework. This step is important to enhance policy impact assessments on e.g., leakage effects.
- Given the fast processing time, we are extending TiMBA to conduct exhaustive uncertainty analysis using Monte Carlo simulations. While these Monte Carlo simulations are currently used in the quantification of carbon stocks, their applications can be deployed to any input data of the model.


Frequently check [TiMBA repository](https://github.com/TI-Forest-Sector-Modelling/TiMBA) for new releases.

## FAIR research software

We attempt to adhere as much as possible to the FAIR Principles for research
software (e.g. see Barker et al. 2022 and Chue Hong et al. 2022) and reach high
standards of scientific quality and openness. I you find TiMBA or its
publication lacking in that regard, we very much appreciate your feedback as an
[Issue](https://github.com/TI-Forest-Sector-Modelling/TiMBA/issues/new/choose)
in the GitHub repository or via email to [wf-timba@thuenen.de](mailto:wf-timba@thuenen.de).

In the following we detail our efforts and considerations.

### Findable

- Zenodo provided DOI with extensive metadata according to the DataCite schema.
  - Separate DOIs for each release and one representing all releases.
- A set of SWHID (Software Hash IDentifiers) is created by the [Software
  Heritage archive](https://archive.softwareheritage.org/) for each release and
  refereneces the code in a very granular fashion.
- An accompanying [software paper](https://doi.org/10.21105/joss.08034) makes
  sure the software is also referenced in search indices that focus on journal articles.
- We provide rich metadata also as Linked Data in the form of the file `codemeta.json` in the
  [CodeMeta](https://codemeta.github.io/) standard.
- We provide machine-actionable citation information in the [Citation File Format
  (CFF)](https://citation-file-format.github.io/).

### Accessible

- The source code can be accessed in the form of a *git remote* from GitHub, via
  *http* from Zenodo, but also by Python package managers through the [Python
  Package Index](https://pypi.org/), both as source distribution and as a
  *Wheel*.

- Metadata is preserved to a very high degree of safety in a redundant fashion.
  The DataCite type metadata not only preserved by DataCite but also by Zenodo
  and in the form of a file `zenodo.json` on GitHub and in the SoftwareHeritage
  datacenters. These three repositories also hold the file `codemeta.json`.

- We are aware that Microsoft GitHub, as a profit driven commercial service, is
  tainted by political and company-strategic limitations, in particular with
  regard to data safety, security, adherence to scientific community-standards
  and long-term viability. However, GitHub's comfortable integration with Zenodo
  yields a lot of benefits that we could not achive otherwise with our limited
  resources. We hope that a comparable integration will be developed in the
  future for better suited Forges such as [Forgejo](https://forgejo.org/) or
  self-hosted [GitLab](gitlab.com).

### Interoperable

#### Software interoperability

- TiMBA installs both as executable script (`timba_run`) as well as as a Python
  module (`TiMBA`). The script copies its output also to STDOUT and allows to
  chain TiMBA with other command line tools in a classical UNIX fashion.
  Importing the module allows users to integrate TiMBA into their own Python
  programs.
  
- We also maintain extensions to TiMBA ([Carbon
  Module](https://github.com/TI-Forest-Sector-Modelling/C-Module) and [TiMBA
  Charts](https://github.com/TI-Forest-Sector-Modelling/TiMBA_Charts)) in the
  form of Python packages that can be used by importing them. These packages
  also adhere to FAIR principles, are version-controlled in GitHub and can be
  referenced with DOIs registered by Zenodo.

#### Data interoperability

- TiMBA relies on a set of required input data. The canonical versions of these
  data (a non-trivial scientific output) are also kept [version controlled in
  GitHub](https://github.com/TI-Forest-Sector-Modelling/TiMBA_Additional_Information)
  and releases are [published to
  Zenodo](https://doi.org/10.5281/zenodo.14928910).

- The main input data file is Office Open XML ("Microsoft Excel") format. We are
  aware that this format is not a good choice from a purely technical point of
  view and has severe shortcomings with regard to robustness, stability over
  time, machine readability, interoperability, platform independence and
  accessibility. However, this is the optimal format to make the input
  understandable and modifyable by our main target group, who understands
  Microsoft Excel very well but might struggle with less common formats and
  conventions. This is a cultural and educational problem we can't solve in this
  context. We are familiar with and track projects such as [Frictionnless
  Data](https://frictionlessdata.io/) with its [Data Package
  standard](https://datapackage.org/). We hope to eventually be able to combine
  user-friendliness with robust data standards to represent our input data.

- TiMBA output is written to universally readable CSV-files. We plan to describe
  these files with standardized metadata in the future.

### Reusable

- TiMBA has dependencies and sub-dependencies which are extensively recorded in
  the `uv.lock` file to ensure reproducibility.
- We continually strife to keep and improve software quality. We utilize
  automated CI processes (testing for different platforms, linting) in the form
  of GitHub workflows.
- We have licensed the software under the [GNU Affero General Public
  License](https://www.gnu.org/licenses/agpl-3.0.txt). On the one hand side this
  license is most compatible with the potential need to In-license further
  dependencies. On the other hand, as a *Copyleft* license, it is very well
  suited to support Open Science, reproducibility and transparency (von Waldow,
  2024).
- The scientific background and in particular prior software on which this work
  improves, is extensively documented and referenced in this README file.
- Python as a programming language, adherence to an improved (`ruff`-defaults)
  PEP 8 style guide, default installation as "editable", user-friendly
  formatting of input data, and instructions about how to contribute increase
  the chances that this software is being reused in our research community.

## Contributing to the project
We welcome contributions, additions and suggestion to further develop or improve the code and the model. To check, discuss and include them into this project, we would like you to share your ideas with us so that we can agree on the requirements needed for accepting your contribution. 
You can contact us directly via GitHub by creating issues, or by writing an Email to:

[wf-timba@thuenen.de](mailto:wf-timba@thuenen.de)

A scientific documentation will follow and be linked here soon. So far, this README serves as a comprehensive introduction and guidance on how to get started. 



## Authors
TiMBA was developed and written by an authors' collective named Thünen Institute Forest Sector Modelling (TI-FSM). 

The individual authors are listed in alphabetical order 
- [Christian Morland](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/ehemalige-liste/christian-morland-msc) [(ORCID 0000-0001-6600-570X)](https://orcid.org/0000-0001-6600-570X), 
- [Franziska Schier](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/dipl-ing-franziska-schier) [(ORCID 0000-0002-3378-1371)](https://orcid.org/0000-0002-3378-1371), 
- [Julia Tandetzki](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/julia-tandetzki-msc) [(ORCID 0000-0002-0630-9434)](https://orcid.org/0000-0002-0630-9434), and 
- [Tomke Honkomp](https://www.thuenen.de/de/fachinstitute/waldwirtschaft/personal/wissenschaftliches-personal/tomke-honkomp-msc) [(ORCID 0000-0002-6719-0190)](https://orcid.org/0000-0002-6719-0190). 

## Contribution statement
Within the authors' collective TI-FSM, the authors have contributed over years their individual strengths and knowledge to make the model work:

| Author            | Conceptualization and theoretical framework | Methodology | Data Curation and Management | Formal Analysis | Programming | Writing and Documentation | Visualization | Review and Editing | Supervision |
|:------------------|:-------------------------------------------:|:-----------:|:----------------------------:|:---------------:|:-----------:|:-------------------------:|:-------------:|:------------------:|:-----------:|
| Christian Morland |                      X                      |      X      |              X               |        X        |      X      |             X             |       X       |         X          |             |
| Franziska Schier  |                      X                      |      X      |              X               |        X        |             |             X             |               |         X          |      X      |
| Julia Tandetzki   |                      X                      |      X      |              X               |        X        |      X      |             X             |       X       |         X          |             |
| Tomke Honkomp     |                      X                      |      X      |              X               |        X        |      X      |             X             |       X       |         X          |             |

## License and copyright note

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



## Acknowledgements

This work is the result of great joint efforts of the forest products market analysis team at the Thünen Institute of Forestry and others from 2018 to 2024. In the last years, many people made important contributions to this work. Without their support, reflection, and constructive criticism, this undertaking would not have been as successful as it turns out to be now. We would like express our gratitude to all of them. In particular, we would like to thank 
-	Pixida GmbH and especially Tobias Hierlmeier for professional support in revising and restructuring the model architecture and code and being valuable help in programming tasks
-	Thünen Institute Service Centre for Research Data Management and especially Harald von Waldow for providing expertise, consultation, and support during the release process
-	Holger Weimar and Matthias Dieter for the trustful and cooperative working environment, valuable support and critical discussion and the opportunity to keep on going
-	Johanna Schliemann for technical support whenever needed
-	The Thünen Institut of Forestry and its Head Matthias Dieter for providing financial resources over the years 
- [makeareadme.com](https://www.makeareadme.com/) for providing the template this README is leaned on.

## References
- Barker, M., Chue Hong, N.P., Katz, D.S. et al. Introducing the FAIR Principles for research software. Sci Data 9, 622 (2022). https://doi.org/10.1038/s41597-022-01710-x
- Buongiorno, J.; Zhu, S.; Zhang, D.; Turner, J.; Tomberlin, D. The Global Forest Products Model; Academic Press: Cambridge, MA, USA, 2003; ISBN 978-0-12-141362-0
- Buongiorno, J. Global modelling to predict timber production and prices: The GFPM approach. Forestry 2015, 88, 291–303.
- Buongiorno, J.; and Zhu, S. 2015. Technical change in forest sector models: The GFPM approach.  Scand. J. For. Research, 30, 30-48.
- GFPM - Global Forest Product Model is available at https://onedrive.live.com/?authkey=%21AEF7RY7oAPlrDPk&id=93BC28B749A1DFB6%21118&cid=93BC28B749A1DFB6
- Chue Hong, N.P. et al. (2022) “FAIR Principles for Research Software (FAIR4RS Principles)”. Zenodo. Available at: https://doi.org/10.15497/RDA00068.
- FAO. Global Forest Resources Assessment: Terms and Definitions; Forest Resources Assessment Working Paper 188; FAO: Rome, Italia, 2020; Available online: http://www.fao.org/3/I8661EN/i8661en.pdf
- FAO. Global Forest Resources Assessment. 2022. Available online: https://fra-data.fao.org/
- FAOSTAT. Forestry Production and Trade: Datenbank. Available online: https://www.fao.org/faostat/en/#data/FO
- Morland, C.; Schier, F.; Janzen, N.; Weimar, H. Supply and demand functions for global wood markets: Specification and plausibility testing of econometric models within the global forest sector. For. Policy Econ. 2018, 92, 92–105
- Morland, C.; Tandetzki, J.; & Honkomp, T. (2025). TiMBA Charts (v0.2.0). Zenodo. https://doi.org/10.5281/zenodo.15689299
- Murray, B.C.; McCarl, B.A.; Lee, H.-C. Estimating Leakage from Forest Carbon Sequestration Programs. Land Econ. 2004, 80, 109–124
- Panayotou, T. Empirical Tests and Policy Analysis of Environmental Degradation at Different Stages of Economic Development; Working Paper No. 238; International Labour Organization: Geneva, Switzerland, 1993; Available online: http://www.ilo.org/public/libdoc/ilo/1993/93B09_31_engl.pdf 
- Riahi, K.; van Vuuren, D.P.; Kriegler, E.; Edmonds, J.; O’Neill, B.C.; Fujimori, S.; Bauer, N.; Calvin, K.; Dellink, R.; Fricko, O.; et al. The Shared Socioeconomic Pathways and their energy, land use, and greenhouse gas emissions implications: An overview. Glob. Environ. Chang. 2017, 42, 153–168.
- Samuelson, Paul A. Spatial Price Equilibrium and Linear Programming; The American Economic Review, 1952, 42 (3), 283–303; Available online http://www.jstor.org/stable/1810381.
- Schier, F.; Morland, C.; Tandetzki, J.; Honkomp, T. (2025). TI-Forest-Sector-Modelling/TiMBA_Additional_Information: Additional Information for TiMBA Setup (v1.0.1). Zenodo. https://doi.org/10.5281/zenodo.14928911.
- TI-FSM (2025) TiMBA - Timber market Model for policy-Based Analysis: Documentation of model structure, data, and parameters. Braunschweig: Johann Heinrich von Thünen-Institut, 35 p, Thünen Working Paper 263, DOI:10.3220/253-2025-16
- von Waldow, H. (2024). Research Software Licensing Guide (Version v1.0.2). Zenodo. https://doi.org/10.5281/zenodo.14008091
- World Bank. World Development Indicators|DataBank. Available online: https://databank.worldbank.org/source/world-development-indicators


