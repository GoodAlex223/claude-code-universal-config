---
name: python-standards
description: Python coding conventions, project structure, type hints, and testing patterns. Use when writing Python code to ensure consistent style, type safety, and best practices.
---

# Python Standards

Python-specific coding standards for Python 3.9+.

---

## Code Style

### Formatting

**Use Black** for formatting:
```bash
black --line-length 88 .
```

Configuration (pyproject.toml):
```toml
[tool.black]
line-length = 88
target-version = ['py39', 'py310', 'py311']
```

### Linting

**Use Ruff** (recommended):
```bash
ruff check .
```

Configuration:
```toml
[tool.ruff]
line-length = 88
select = ["E", "F", "W", "I", "N", "UP", "B", "C4"]

[tool.ruff.isort]
known-first-party = ["my_package"]
```

### Import Order

```python
# 1. Standard library
import os
import sys
from typing import Dict, List, Optional

# 2. Third-party packages
import requests
from pydantic import BaseModel

# 3. Local application
from my_package.utils import helper
from my_package.models import User
```

---

## Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Modules | snake_case | `user_service.py` |
| Classes | PascalCase | `UserService` |
| Functions | snake_case | `get_user_by_id` |
| Variables | snake_case | `user_count` |
| Constants | UPPER_SNAKE | `MAX_RETRIES` |
| Private | _leading_underscore | `_internal_method` |

```python
# Good
user_repository = UserRepository()
is_valid = validate_input(data)
MAX_RETRY_ATTEMPTS = 3

# Bad
ur = UserRepository()  # Too short
isvalid = validate_input(data)  # Missing underscore
maxRetryAttempts = 3  # Wrong convention
```

---

## Type Hints

### Always Use Type Hints

```python
from typing import Optional
from collections.abc import Sequence

def get_user(user_id: int) -> Optional[User]:
    """Fetch user by ID."""
    ...

def process_items(items: Sequence[str]) -> dict[str, int]:
    """Process items and return counts."""
    ...
```

### Modern Syntax (3.10+)

```python
def get_user(user_id: int) -> User | None:
    ...

def process(data: list[str]) -> dict[str, int]:
    ...
```

### Type Checking

**Use mypy**:
```bash
mypy --strict src/
```

Configuration:
```toml
[tool.mypy]
python_version = "3.11"
strict = true
warn_return_any = true
disallow_untyped_defs = true
```

---

## Project Structure

```
project/
├── pyproject.toml
├── README.md
├── src/
│   └── my_package/
│       ├── __init__.py
│       ├── __main__.py
│       ├── models/
│       ├── services/
│       └── utils/
├── tests/
│   ├── conftest.py
│   ├── unit/
│   └── integration/
└── docs/
```

### `__init__.py`

```python
from my_package.models.user import User
from my_package.services.user_service import UserService

__all__ = ["User", "UserService"]
__version__ = "1.0.0"
```

---

## Dependencies (pyproject.toml)

```toml
[project]
name = "my-package"
version = "1.0.0"
requires-python = ">=3.9"
dependencies = [
    "requests>=2.28.0,<3.0.0",
    "pydantic>=2.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "pytest-cov>=4.0.0",
    "mypy>=1.0.0",
    "ruff>=0.1.0",
]
```

---

## Error Handling

### Custom Exceptions

```python
class AppError(Exception):
    """Base application error."""

    def __init__(self, message: str, code: str | None = None):
        super().__init__(message)
        self.code = code


class ValidationError(AppError):
    """Invalid input data."""
    pass


class NotFoundError(AppError):
    """Resource not found."""
    pass
```

### Exception Handling

```python
# Good: Catch specific exceptions
try:
    user = get_user(user_id)
except NotFoundError:
    logger.warning(f"User {user_id} not found")
    return None
except DatabaseError as e:
    logger.error(f"Database error: {e}")
    raise

# Bad: Too broad
try:
    user = get_user(user_id)
except Exception:  # Too broad
    pass  # Silent failure
```

---

## Testing

### pytest Configuration

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-v --cov=src --cov-report=term-missing"
```

### Test Structure

```python
import pytest
from my_package.services import UserService


class TestUserService:
    @pytest.fixture
    def service(self) -> UserService:
        return UserService()

    def test_get_user_returns_user(self, service: UserService) -> None:
        # Arrange
        user_id = 123

        # Act
        result = service.get_user(user_id)

        # Assert
        assert result is not None
        assert result.id == user_id
```

### Fixtures

```python
# conftest.py
@pytest.fixture(scope="session")
def engine():
    return create_engine("sqlite:///:memory:")

@pytest.fixture
def db_session(engine) -> Session:
    connection = engine.connect()
    transaction = connection.begin()
    session = Session(bind=connection)

    yield session

    session.close()
    transaction.rollback()
    connection.close()
```

---

## Docstrings (Google Style)

```python
def calculate_score(
    items: list[Item],
    weights: dict[str, float],
    normalize: bool = True,
) -> float:
    """Calculate weighted score for items.

    Args:
        items: List of items to score.
        weights: Mapping of item types to weights.
        normalize: Whether to normalize the score.

    Returns:
        The calculated score.

    Raises:
        ValueError: If items list is empty.
    """
```

---

## Common Patterns

### Context Managers

```python
from contextlib import contextmanager

@contextmanager
def database_transaction():
    session = Session()
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()
```

### Data Classes

```python
from dataclasses import dataclass, field
from datetime import datetime

@dataclass
class User:
    id: int
    email: str
    created_at: datetime = field(default_factory=datetime.utcnow)
```

### Pydantic Models

```python
from pydantic import BaseModel, Field, EmailStr

class UserCreate(BaseModel):
    email: EmailStr
    name: str = Field(min_length=1, max_length=100)

    model_config = {"strict": True}
```

---

## Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| `from module import *` | Pollutes namespace | Import specific names |
| Mutable default args | Shared state bugs | Use `None` default |
| Bare `except:` | Catches everything | Catch specific exceptions |
| Global variables | Hard to test | Dependency injection |

```python
# Bad: Mutable default
def add_item(item, items=[]):
    items.append(item)

# Good: None default
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
```

---

## Tools Summary

| Tool | Purpose | Command |
|------|---------|---------|
| Black | Formatting | `black .` |
| Ruff | Linting | `ruff check .` |
| mypy | Type checking | `mypy --strict .` |
| pytest | Testing | `pytest` |
| pip-audit | Security | `pip-audit` |
