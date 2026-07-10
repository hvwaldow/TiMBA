# TODO — Migrate TiMBA to uv

This list tracks the work to manage the project with [uv](https://docs.astral.sh/uv/) while keeping it portable across Linux, Windows, and macOS.

Decisions already taken:
- `requirements.txt` is kept as a fallback for non-uv users.
- CI workflows are updated to use uv.
- Supported Python stays `3.9–3.11`; the project itself is pinned to `3.11` via `.python-version`.
- `AGENTS.md` and `CONTRIBUTING.md` are updated to reflect the new commands.

## Steps

1. [x] **Install and set up uv**
   - Document the cross-platform uv install command in `README.md`.
   - Verify uv is available and can install Python 3.11.

2. [x] **Move dependency management to uv**
   - Use `pyproject.toml` as the single source of truth.
   - Keep `requirements.txt` as a fallback for pip users.
   - Run `uv lock` to generate a reproducible `uv.lock` file.
   - Keep `coverage` in the main dependency list (no `dev` group needed for now).

3. [x] **Pin the project Python version**
   - Add a `.python-version` file containing `3.11`.
   - This ensures local development and the default uv workflow use the same interpreter.

4. [x] **Clean up `pyproject.toml` metadata**
   - Fix the trailing space in `requires-python`.
   - Make classifiers consistent with the supported Python range.
   - Keep the build backend as `setuptools` (works with uv).

5. [x] **Verify Linux install with uv**
   - `uv sync`
   - `uv run run_timba --help`
   - `uv run run_timba -MP=1`
   - `uv run python -W ignore::DeprecationWarning -m coverage run --rcfile=.coveragerc -m unittest discover`
   - `uv run coverage report` (85% coverage)

6. [ ] **Verify Windows install with uv**
   - Run `uv sync` and the same commands on Windows.
   - Adjust any path-related issues if they appear.

7. [ ] **Verify macOS install with uv**
   - `uv python install 3.11`
   - `uv sync`
   - Run the test/model commands.
   - Investigate and resolve any OSQP/cvxpy/numerical differences that currently make macOS unsupported.

8. [x] **Update `README.md` with the uv workflow**
   - Add a “Quick start with uv” section.
   - Document the main commands: `uv sync`, `uv run run_timba`, and running tests.
   - Keep the existing `pip install timba` quick-start for users who only want the package from PyPI.

9. [x] **Update CI/CD to use uv**
   - Switch `.github/workflows/actions.yml` to use `uv sync` / `uv run`.
   - Switch `.github/workflows/sonarscan.yml` to use `uv sync` / `uv run`.
   - Extend the CI matrix to include `macos-latest` for all tested Python versions.

10. [x] **Update developer docs**
    - Update `AGENTS.md` with the new install/test commands.
    - Update `CONTRIBUTING.md` with the new developer workflow.
    - Update `CHANGELOG.md` with the uv migration note.

11. [x] **Final build validation**
    - Build a wheel with `uv build --sdist --wheel` and verify it builds correctly.
