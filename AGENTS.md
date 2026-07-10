# AGENTS.md — TiMBA Agent Guidance

This file contains the information needed by coding agents to work safely and effectively on the TiMBA repository.

## Project Overview

TiMBA (Timber market Model for policy-Based Analysis) is a Python package implementing a partial economic equilibrium model for the global forest products market. It is published on PyPI as `pytimba` and developed by the Thünen Institute Forest Sector Modelling (TI-FSM).

- **Repository**: `https://github.com/TI-Forest-Sector-Modelling/TiMBA`
- **License**: AGPL-3.0-or-later (see `COPYING` and `COPYRIGHT`)
- **Package name**: `pytimba` on PyPI; import name is `TiMBA` (capitalized)
- **Documentation**: `documentation/documentation_TiMBA.pdf`

## Supported Environment

- **Python**: `>=3.9,<3.12` (CI tests 3.9, 3.10, 3.11 on Linux, Windows, and macOS)
- **Operating systems**: Linux, Windows, and macOS are supported. Python 3.12/3.13 are currently not supported.
- **Default Python version**: The project is pinned to Python 3.11 in `.python-version`.
- **Dependency files**: `pyproject.toml`, `requirements.txt`, and `uv.lock` must be kept in sync.

## Setup and Install

The project uses [uv](https://docs.astral.sh/uv/) for dependency management. For local development:

```bash
# Install uv (one of the following)
curl -LsSf https://astral.sh/uv/install.sh | sh  # Linux / macOS
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"  # Windows
# or: pip install uv

# Clone the repository
git clone https://github.com/TI-Forest-Sector-Modelling/TiMBA.git
cd TiMBA

# Create the virtual environment and install the project
uv sync
```

Verify the installation:

```bash
uv run run_timba --help
uv run run_timba -MP=1
```

If you prefer a pip-based workflow, you can still use `requirements.txt`:

```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
python -m pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
pip install -e .
```

## Build and Test

### Run the test suite

```bash
uv run python -W ignore::DeprecationWarning -m coverage run --rcfile=.coveragerc -m unittest discover
uv run coverage report
uv run coverage xml
```

- Coverage is configured in `.coveragerc` and `pyproject.toml` (`fail_under = 50`).
- CI runs the same command on Linux, Windows, and macOS for Python 3.9–3.11.
- Tests live in `test/` and must not be included in coverage (`.coveragerc` omits `test/*`).

### Build the package

```bash
uv build --sdist --wheel
```

## Project Structure

```
TiMBA/
├── TiMBA/                      # Main package (import as `TiMBA`)
│   ├── main.py                 # Public API: run_timba(), parameter_setter()
│   ├── main_runner/            # Orchestrates input loading, model run, output
│   ├── cli/                    # Click-based CLI commands
│   ├── data_management/        # Data loading, containers, serialization, parameters
│   ├── data_validation/        # Result validation helpers
│   ├── logic/                  # Core model, optimization, extensions
│   ├── parameters/             # Constants, enums, paths, domains
│   ├── user_io/                # Default parameters and user input
│   ├── results_logging/        # Logging and output writing
│   └── helpers/                # Utility functions
├── test/                       # Unit tests
├── documentation/              # PDF documentation
├── paper/                      # JOSS paper sources
├── pyproject.toml              # Build and project metadata
├── uv.lock                     # Locked dependency tree
├── requirements.txt            # Pinned runtime dependencies (fallback for pip users)
├── .python-version             # Default Python version for uv
├── .coveragerc                 # Coverage configuration
└── .github/workflows/          # CI/CD workflows
```

## Coding Conventions

- Follow **PEP 8**.
- Add docstrings for all new functions and classes.
- Import internal modules using the full package name: `from TiMBA.<module> import ...`.
- Use `pathlib.Path` for paths; avoid hard-coded platform-specific separators.
- Keep existing validated model settings intact. If you change settings, document them in `README.md` and `CHANGELOG.md`.
- Do not commit generated data, outputs, or virtual environments (see `.gitignore`).
- `COPYING` and `COPYRIGHT` must not be removed.

## Important Behavioral Constraints

- The model was validated only for a subset of setting combinations. In particular, `calc_product_price` (PP) and `calc_world_price` (WP) modes must be applied consistently; mixing shadow and calculated modes is not supported.
- Only the first nine periods have been tested. Changing `max_period` may require corresponding changes in the `ExogChange` sheet of the input file.
- Carbon module functionality is provided by the `Carbon-Module` package and is activated via `default_parameters.py` or the CLI `-C` flag.

## Testing and Validation

- Add unit tests for new features or fixes in `test/`.
- The main integration test (`test/test_TiMBA.py`) runs TiMBA for a single period and compares results against a reference pickle with a relative tolerance of 5%.
- External data is downloaded automatically from the `TiMBA_Additional_Information` GitHub repository via `load_data()` and `load_timba` CLI command.

## CI/CD

- `.github/workflows/actions.yml` runs the full test suite with coverage using `uv`.
- `.github/workflows/sonarscan.yml` uploads coverage and runs SonarCloud analysis using `uv`.
- `.github/workflows/publish.yml` builds and publishes the package to PyPI when a version tag `v*` is pushed.

## Common Agent Tasks

- **Bug fix**: locate the issue, add or update a test, verify with `uv run python -m unittest discover`, then run the full coverage suite.
- **Feature**: add a focused test, keep changes minimal, regenerate `uv.lock` with `uv lock`, and update `README.md` and `CHANGELOG.md` if user-facing behavior changes.
- **Dependencies**: update `pyproject.toml`, regenerate `uv.lock`, and keep `requirements.txt` in sync when adding or changing packages.
- **Documentation**: keep `README.md` accurate. Avoid creating separate documentation files unless explicitly requested.

## Notes for Agents

- Do not run `git commit`, `git push`, `git reset`, or other git mutations unless explicitly requested.
- Do not modify validated scientific defaults without clear justification and user confirmation.
- When modifying settings, ensure the CLI (`TiMBA/cli/cli.py`) and `default_parameters.py` remain consistent.
- Keep the package importable name `TiMBA` unchanged; the PyPI distribution name is `pytimba`.
