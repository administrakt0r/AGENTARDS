# Python Stack Template

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
