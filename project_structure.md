# 📁 Повна структура проекту Contacts API (2024)

Сучасна архітектура REST API з використанням найновіших практик Python розробки.

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com)
[![Architecture](https://img.shields.io/badge/Architecture-Clean-brightgreen.svg)](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

## 🏗️ Архітектурна структура

```
contacts-api/
├── 📂 app/                           # 🎯 Основний код додатку (Clean Architecture)
│   ├── __init__.py                  # Ініціалізація пакету
│   ├── main.py                      # 🚀 FastAPI додаток та middleware
│   ├── config.py                    # ⚙️ Конфігурація та налаштування
│   ├── dependencies.py              # 🔗 Залежності FastAPI
│   │
│   ├── 📂 core/                     # 🔧 Основні компоненти
│   │   ├── __init__.py
│   │   ├── database.py              # 🗄️ SQLAlchemy налаштування
│   │   ├── security.py              # 🔐 Безпека та авторизація
│   │   └── exceptions.py            # ⚠️ Кастомні винятки
│   │
│   ├── 📂 models/                   # 📊 Моделі бази даних
│   │   ├── __init__.py
│   │   ├── base.py                  # Базова модель
│   │   └── contact.py               # Модель контакту
│   │
│   ├── 📂 schemas/                  # 📋 Pydantic схеми
│   │   ├── __init__.py
│   │   ├── base.py                  # Базові схеми
│   │   ├── contact.py               # Схеми контактів
│   │   └── response.py              # Схеми відповідей
│   │
│   ├── 📂 services/                 # 🔄 Бізнес-логіка (Service Layer)
│   │   ├── __init__.py
│   │   ├── base.py                  # Базовий сервіс
│   │   └── contact_service.py       # Сервіс контактів
│   │
│   ├── 📂 repositories/             # 📝 Репозиторії (Data Access Layer)
│   │   ├── __init__.py
│   │   ├── base.py                  # Базовий репозиторій
│   │   └── contact_repository.py    # Репозиторій контактів
│   │
│   ├── 📂 api/                      # 🌐 API роутери (Presentation Layer)
│   │   ├── __init__.py
│   │   ├── deps.py                  # API залежності
│   │   └── v1/                      # API версія 1
│   │       ├── __init__.py
│   │       ├── api.py               # Головний роутер v1
│   │       └── endpoints/
│   │           ├── __init__.py
│   │           ├── contacts.py      # Endpoints контактів
│   │           └── health.py        # Health check endpoints
│   │
│   └── 📂 utils/                    # 🛠️ Утиліти та допоміжні функції
│       ├── __init__.py
│       ├── validators.py            # Кастомні валідатори
│       ├── formatters.py            # Форматування даних
│       └── constants.py             # Константи додатку
│
├── 📂 tests/                        # 🧪 Тестування
│   ├── __init__.py
│   ├── conftest.py                  # Pytest конфігурація та фікстури
│   ├── test_main.py                 # Тести основного додатку
│   │
│   ├── 📂 unit/                     # Юніт тести
│   │   ├── __init__.py
│   │   ├── test_services/
│   │   │   ├── __init__.py
│   │   │   └── test_contact_service.py
│   │   ├── test_repositories/
│   │   │   ├── __init__.py
│   │   │   └── test_contact_repository.py
│   │   └── test_utils/
│   │       ├── __init__.py
│   │       └── test_validators.py
│   │
│   ├── 📂 integration/              # Інтеграційні тести
│   │   ├── __init__.py
│   │   └── test_api/
│   │       ├── __init__.py
│   │       ├── test_contacts_api.py
│   │       └── test_health_api.py
│   │
│   └── 📂 fixtures/                 # Тестові дані та фікстури
│       ├── __init__.py
│       ├── contact_fixtures.py
│       └── database_fixtures.py
│
├── 📂 scripts/                      # 📜 Скрипти автоматизації
│   ├── __init__.py
│   ├── seed_data.py                 # 🎭 Генерація тестових даних
│   ├── backup_db.py                 # 💾 Резервне копіювання БД
│   ├── restore_db.py                # 🔄 Відновлення БД
│   ├── run_migrations.py            # 🔄 Запуск міграцій
│   └── setup_dev.py                 # 🛠️ Налаштування dev середовища
│
├── 📂 migrations/                   # 🔄 Alembic міграції (оновлена структура)
│   ├── versions/                    # Файли міграцій
│   ├── env.py                       # Alembic environment
│   ├── script.py.mako               # Шаблон міграцій
│   └── README.md                    # Документація міграцій
│
├── 📂 docs/                         # 📚 Документація проекту
│   ├── api/                         # API документація
│   │   ├── endpoints.md
│   │   └── examples.md
│   ├── development/                 # Документація розробки
│   │   ├── setup.md
│   │   ├── testing.md
│   │   └── deployment.md
│   └── architecture/                # Архітектурна документація
│       ├── overview.md
│       └── database.md
│
├── 📂 docker/                       # 🐳 Docker конфігурації
│   ├── Dockerfile                   # Production образ
│   ├── Dockerfile.dev               # Development образ
│   ├── docker-compose.yml           # Основний compose
│   ├── docker-compose.dev.yml       # Development compose
│   ├── docker-compose.prod.yml      # Production compose
│   └── docker-compose.test.yml      # Testing compose
│
├── 📂 .github/                      # 🔄 GitHub workflows та шаблони
│   ├── workflows/
│   │   ├── ci.yml                   # Continuous Integration
│   │   ├── cd.yml                   # Continuous Deployment
│   │   └── tests.yml                # Automated Testing
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── pull_request_template.md
│
├── 📂 monitoring/                   # 📊 Моніторинг та логування
│   ├── logging_config.py
│   ├── metrics.py
│   └── health_checks.py
│
├── 📂 deployment/                   # 🚀 Конфігурації розгортання
│   ├── kubernetes/                  # K8s маніфести
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   ├── terraform/                   # Infrastructure as Code
│   │   ├── main.tf
│   │   └── variables.tf
│   └── ansible/                     # Ansible playbooks
│       └── deploy.yml
│
├── 🔧 .env                          # Змінні середовища (не в git)
├── 🔧 .env.example                  # Приклад змінних середовища
├── 🔧 .env.test                     # Тестові змінні середовища
├── 🔧 .env.production               # Продакшн змінні (шаблон)
│
├── ⚙️ pyproject.toml                # Poetry конфігурація + інструменти
├── 🔒 poetry.lock                   # Lock файл Poetry
├── 📦 requirements.txt              # Pip requirements (альтернатива)
├── 📦 requirements-dev.txt          # Development requirements
│
├── 🗄️ alembic.ini                   # Alembic конфігурація
├── 🧪 pytest.ini                    # Pytest конфігурація
├── 🎨 .pre-commit-config.yaml       # Pre-commit hooks
├── 🔍 .editorconfig                 # Editor конфігурація
│
├── 🐳 .dockerignore                 # Docker ignore
├── 📝 .gitignore                    # Git ignore
├── 📄 .gitattributes                # Git attributes
│
├── 📋 Makefile                      # Автоматизація команд
├── 📜 justfile                      # Just command runner (сучасна альтернатива Make)
│
├── 📚 README.md                     # Головна документація
├── 📄 CHANGELOG.md                  # Історія змін
├── 📜 LICENSE                       # Ліцензія
├── 🤝 CONTRIBUTING.md               # Інструкції для контрибуторів
├── 🔒 SECURITY.md                   # Політика безпеки
└── 📊 ROADMAP.md                    # Дорожна карта проекту
```

## 🏛️ Архітектурні принципи

### 🎯 Clean Architecture

Проект організований за принципами Clean Architecture:

1. **📊 Domain Layer** (`models/`, `schemas/`) - бізнес-логіка та сутності
2. **🔄 Application Layer** (`services/`) - use cases та бізнес-правила  
3. **📝 Infrastructure Layer** (`repositories/`, `core/`) - зовнішні залежності
4. **🌐 Presentation Layer** (`api/`) - контролери та API endpoints

### 🔧 Dependency Injection

- **Залежності** визначені в `dependencies.py` та `api/deps.py`
- **Інверсія контролю** через FastAPI Depends
- **Тестування** через mock dependencies

### 🔀 Repository Pattern

- **Абстракція** доступу до даних
- **Тестування** незалежно від БД
- **Гнучкість** зміни джерел даних

## 📂 Детальний опис директорій

### `/app/core` - Основні компоненти

```python
# app/core/database.py - Сучасне налаштування SQLAlchemy 2.0+
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine
from sqlalchemy.orm import sessionmaker

# app/core/security.py - JWT, OAuth2, Rate Limiting
from fastapi_limiter import FastAPILimiter
from passlib.context import CryptContext

# app/core/exceptions.py - Кастомні винятки
class ContactNotFoundError(Exception):
    pass
```

### `/app/services` - Бізнес-логіка

```python
# app/services/contact_service.py
class ContactService:
    def __init__(self, repository: ContactRepository):
        self.repository = repository
    
    async def get_upcoming_birthdays(self, days: int = 7) -> List[Contact]:
        # Бізнес-логіка для днів народження
        pass
```

### `/app/repositories` - Доступ до даних

```python
# app/repositories/contact_repository.py
class ContactRepository(BaseRepository[Contact]):
    async def find_by_email(self, email: str) -> Optional[Contact]:
        # Логіка пошуку в БД
        pass
```

### `/tests` - Сучасне тестування

```bash
tests/
├── unit/           # Швидкі тести окремих компонентів
├── integration/    # Тести взаємодії компонентів
├── e2e/           # End-to-end тести (опціонально)
└── fixtures/      # Повторно використовувані тестові дані
```

### `/docker` - Контейнеризація

```yaml
# docker/docker-compose.dev.yml - Development
services:
  web:
    build:
      context: ..
      dockerfile: docker/Dockerfile.dev
    volumes:
      - ..:/app
    environment:
      - DEBUG=True
```

### `/.github` - CI/CD Pipeline

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: pytest
```

## 🛠️ Сучасні інструменти розробки

### 📦 Управління залежностями

```toml
# pyproject.toml - Все в одному файлі
[tool.poetry]
name = "contacts-api"
version = "0.1.0"

[tool.poetry.dependencies]
python = "^3.11"
fastapi = "^0.104.1"
sqlalchemy = {extras = ["asyncio"], version = "^2.0.23"}

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py"]
python_classes = ["Test*"]

[tool.black]
line-length = 88
target-version = ['py311']

[tool.isort]
profile = "black"
multi_line_output = 3

[tool.mypy]
python_version = "3.11"
warn_return_any = true
strict_optional = true
```

### 🎨 Якість коду

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/psf/black
    rev: 23.3.0
    hooks:
      - id: black
  
  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
  
  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
  
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.3.0
    hooks:
      - id: mypy
```

### 🚀 Автоматизація

```makefile
# Makefile - Сучасні команди
.PHONY: help install test lint format clean docker-up

help:  ## Показати допомогу
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

install:  ## Встановити залежності
	poetry install --with dev,test

test:  ## Запустити тести
	poetry run pytest -v --cov=app --cov-report=html

lint:  ## Перевірити код
	poetry run black --check app tests
	poetry run isort --check-only app tests
	poetry run flake8 app tests
	poetry run mypy app

format:  ## Форматувати код
	poetry run black app tests
	poetry run isort app tests

docker-up:  ## Запустити через Docker
	docker-compose -f docker/docker-compose.dev.yml up -d

clean:  ## Очистити тимчасові файли
	find . -type d -name "__pycache__" -delete
	find . -type f -name "*.pyc" -delete
	rm -rf .pytest_cache htmlcov .coverage
```

```just
# justfile - Сучасна альтернатива Makefile
# https://github.com/casey/just

# Показати всі команди
default:
    @just --list

# Встановити залежності
install:
    poetry install --with dev,test

# Запустити тести з покриттям
test:
    poetry run pytest -v --cov=app --cov-report=html --cov-report=term

# Перевірити якість коду
lint:
    poetry run black --check app tests
    poetry run isort --check-only app tests  
    poetry run flake8 app tests
    poetry run mypy app

# Форматувати код
format:
    poetry run black app tests
    poetry run isort app tests

# Запустити pre-commit hooks
pre-commit:
    poetry run pre-commit run --all-files

# Запустити розробницьке середовище
dev:
    docker-compose -f docker/docker-compose.dev.yml up -d
    poetry run uvicorn app.main:app --reload --host 0.0.0.0

# Створити міграцію
migration message:
    poetry run alembic revision --autogenerate -m "{{message}}"

# Застосувати міграції
migrate:
    poetry run alembic upgrade head

# Створити тестові дані
seed:
    poetry run python scripts/seed_data.py

# Повна настройка для нового розробника
setup: install
    cp .env.example .env
    just dev
    sleep 10
    just migrate
    just seed
    @echo "🎉 Середовище готове! API: http://localhost:8000/docs"
```

## 🔧 Поетапне створення проекту

### 1️⃣ Ініціалізація проекту

```bash
# Створити директорію та ініціалізувати Git
mkdir contacts-api && cd contacts-api
git init

# Ініціалізувати Poetry
poetry init --name contacts-api --description "Modern FastAPI contacts API"

# Створити базову структуру
mkdir -p {app/{core,models,schemas,services,repositories,api/v1/endpoints,utils},tests/{unit,integration,fixtures},scripts,migrations,docs,docker,.github/workflows}

# Створити __init__.py файли
find app tests scripts -type d -exec touch {}/__init__.py \;
```

### 2️⃣ Налаштування інструментів

```bash
# Встановити основні залежності
poetry add fastapi uvicorn sqlalchemy psycopg2-binary pydantic python-dotenv alembic

# Встановити dev залежності
poetry add --group dev pytest pytest-asyncio pytest-cov black isort flake8 mypy pre-commit

# Ініціалізувати pre-commit
poetry run pre-commit install
```

### 3️⃣ Створення основних файлів

```bash
# Створити конфігураційні файли
touch .env.example .gitignore .dockerignore pytest.ini .pre-commit-config.yaml
touch Makefile justfile CHANGELOG.md CONTRIBUTING.md SECURITY.md

# Створити Docker файли
touch docker/{Dockerfile,Dockerfile.dev,docker-compose.yml,docker-compose.dev.yml}

# Створити GitHub workflows
touch .github/workflows/{ci.yml,cd.yml}
```

## 🎯 Переваги сучасної структури

### ✅ Масштабованість
- **Модульна архітектура** дозволяє легко додавати нові функції
- **Separation of Concerns** - кожен компонент має свою відповідальність
- **Dependency Injection** забезпечує гнучкість

### ✅ Тестованість
- **Юніт тести** для бізнес-логіки
- **Інтеграційні тести** для API
- **Високе покриття** коду тестами

### ✅ Якість коду
- **Автоматичне форматування** (Black, isort)
- **Статичний аналіз** (mypy, flake8)
- **Pre-commit hooks** для контролю якості

### ✅ DevOps Ready
- **CI/CD pipeline** з GitHub Actions
- **Docker** для контейнеризації
- **Infrastructure as Code** з Terraform

### ✅ Документація
- **Автоматична API документація** (OpenAPI/Swagger)
- **Архітектурна документація** в `/docs`
- **Інструкції для контрибуторів**

## 🚀 Швидкий старт з сучасною структурою

```bash
# 1. Клонувати та встановити
git clone <repo> && cd contacts-api
just setup  # або make setup

# 2. Перевірити що все працює
just test
just lint

# 3. Запустити розробницьке середовище
just dev

# 4. Відкрити документацію
open http://localhost:8000/docs
```

## 📊 Порівняння зі старою структурою

| Аспект | Стара структура | Сучасна структура |
|--------|-----------------|-------------------|
| **Архітектура** | Monolithic | Clean Architecture |
| **Тестування** | Базове | Комплексне (unit/integration) |
| **CI/CD** | Відсутнє | GitHub Actions |
| **Якість коду** | Ручна | Автоматична (pre-commit) |
| **Документація** | Мінімальна | Повна |
| **Масштабованість** | Обмежена | Висока |
| **Maintainability** | Середня | Висока |

## 🎯 Результат

Така структура забезпечує:

1. **🔧 Легкість розробки** - зрозуміла архітектура
2. **🧪 Високу якість** - автоматичні перевірки
3. **🚀 Швидке розгортання** - готові CI/CD
4. **📈 Масштабованість** - модульний дизайн
5. **🤝 Командну роботу** - стандартизовані процеси

---

**💡 Ця структура готова для enterprise-рівня розробки та може служити шаблоном для інших проектів!**