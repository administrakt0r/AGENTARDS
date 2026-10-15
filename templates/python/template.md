# Python Stack Template


## Stack Detection Signals

```bash
rg --files -g 'pyproject.toml' -g 'requirements*.txt' -g 'manage.py' -g 'pytest.ini' -g '!**/__pycache__/**' -g '!.venv' -g '!venv'
rg -n 'fastapi|django|flask|pytest' pyproject.toml requirements*.txt -g '*.py' -g '!.venv' -g '!venv'
```

## Detected Technologies
- **Framework:** [Django/Flask/FastAPI/Typer/etc.]
- **Package Manager:** [pip/poetry/pipenv/conda/uv/etc.]
- **Test Runner:** [pytest/unittest/tox/nox/etc.]
- **Linter:** [ruff/flake8/pylint/black/isort/mypy/etc.]
- **Build Tool:** [setuptools/flit/poetry/hatch/etc.]

## Common Stack Patterns

### Project Structure
```
src/
├── __init__.py
├── main.py or app.py
├── models/
├── views/ or routes/
├── services/
├── utils/
├── config.py
└── types/
tests/
├── test_*.py
├── conftest.py
└── fixtures/
```

### Package Management
- pip: `requirements.txt`, `requirements-dev.txt`
- Poetry: `pyproject.toml` with `[tool.poetry]`
- uv: `pyproject.toml` with `[project]`
- Conda: `environment.yml`

### Type Checking
- mypy: `mypy.ini` or `pyproject.toml [tool.mypy]`
- pyright: `pyrightconfig.json`
- Type hints: `typing` module, Python 3.10+ syntax

### Testing
- pytest: `test_*.py`, `conftest.py`, fixtures
- Coverage: `coverage run`, `pytest-cov`
- Mocking: `unittest.mock`, `pytest-mock`
- Async: `pytest-asyncio`

### Code Quality
- Formatting: black, ruff format
- Import sorting: isort, ruff
- Linting: ruff, flake8, pylint
- Type checking: mypy, pyright

## Project Type Detection Signals
- `pyproject.toml` with Python project metadata
- `setup.py` or `setup.cfg`
- `requirements.txt`
- `manage.py` (Django)
- `src/` with `__init__.py`
- `.python-version`

## Agent Customizations

### Python-Specific
- Virtual environments: venv, virtualenv, conda
- Entry points: `console_scripts` in pyproject.toml
- Logging: `logging` module configuration
- Async: asyncio, aiohttp, httpx

### Django
- Models in `models.py`
- Views: function-based or class-based
- URLs: `urlpatterns` in `urls.py`
- Migrations: `makemigrations`, `migrate`

### FastAPI
- Pydantic models for validation
- Dependency injection: `Depends()`
- Async endpoints
- Auto-generated OpenAPI docs

### Packaging
- sdist vs wheel
- Version management
- Entry points and console scripts
- Package data and data files

## Common Issues
- No pinned/isolated environment (`requirements.txt` drift)
- Blocking I/O in async code / GIL-bound hot paths
- Mutable default arguments and shared state
- Import cycles and heavy import-time side effects
- Missing type hints / mypy failures
- Bare `except:` swallowing errors

## Testing Patterns
- `pytest` (with `-q` and coverage)
- `unittest` for stdlib-only projects
- Property tests: Hypothesis
- Lint/type: `ruff`, `mypy`
- `tox` / `nox` for multi-version matrices

## Industry Standards (verified 2026-10 via Context7)

Sources: `docs.python.org/3/tutorial/`, `docs.pytest.org/en/stable/`, `docs.astral.sh/ruff/`, `fastapi.tiangolo.com/`, `docs.djangoproject.com/en/stable/`

- Use the environment tool the repo documents (`venv`, Poetry, uv, pip). Do not add another.
- Type-check with mypy or Pyright only when that tool is already configured.
- Async handlers stay non-blocking. ORM queries that loop over relations are N+1 leads, confirmed by the query count.
- `pytest` is the default runner when `pyproject.toml` or `pytest.ini` says so.
