# ADR 4. Pixi Support
## Context
- The install docs (`doc/install.rst`) are being rewritten (PR ###), with categories: project-based (uv), then environment-based (pip + venv, conda).
- pixi was planned as a second project-based tool, next to uv.
- Elephant is not on conda-forge; pixi installs it from PyPI (`pixi add --pypi elephant`).
- Dependencies and extras are `dynamic` in `pyproject.toml`, read from `requirements/*.txt`:
```toml
dynamic = ["version", "dependencies", "optional-dependencies"]
optional-dependencies.extras = {file = "requirements/requirements-extras.txt"}
```
- pixi has no CLI equivalent of `pip install ".[extras,docs]"` / `uv sync --extra docs`. Every extras group and combination must be defined in `[tool.pixi.*]` as a feature plus a named environment (`pixi run -e docs ...`).
- Static dependencies would shorten this, but not remove it.

## Options
|     | Option | Pro | Con |
| --- | ------ | --- | --- |
| 1   | Full support: `[tool.pixi.*]` in `pyproject.toml` + pixi tabs in the docs | 1. `pixi install` works out of the box<br>2. conda-forge packages (MPI, pandoc, pycuda) | 1. Bloats up pyproject.toml<br>2. Another extra thing to keep in sync|
| 2   | Mention pixi in docs only: pixi tab in "Getting Elephant", no config in the repo | 1. No maintenance in `pyproject.toml` | 1. Looks wierd in only one place |
| 3   | Leave pixi out for now | 1. Nothing extra to maintain<br>2. uv already covers the project-based workflow | 1. pixi users follow the pixi docs themselves |

## Decision
Accepted: Option 3
- No `[tool.pixi.*]` in `pyproject.toml`, no `pixi.toml`, no `pixi.lock`.
- No pixi tabs or mentions in `doc/install.rst` or `doc/contribute.rst`.
- Project-based workflow: uv only. conda stays as the conda-forge route (MPI, OpenCL).

## Discussion
- Any other suggestions ?
