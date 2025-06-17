# 📱 Contacts REST API

REST API для управління контактами, побудований з використанням **FastAPI**, **SQLAlchemy** та **PostgreSQL**.

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-blue.svg)](https://postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://docker.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 🚀 Особливості

- ✅ **Повні CRUD операції** - створення, читання, оновлення та видалення контактів
- 🔍 **Розширений пошук** - за ім'ям, прізвищем або email адресою
- 🎂 **Дні народження** - автоматичне відстеження контактів з ДН на найближчі 7 днів
- ✨ **Валідація даних** - строга перевірка вхідних даних з Pydantic
- 📚 **Автоматична документація** - Swagger UI та ReDoc з інтерактивними прикладами
- 🐳 **Docker підтримка** - повна контейнеризація з Docker Compose
- 🎭 **Тестові дані** - генерація реалістичних українських даних з Faker
- 🧪 **Покриття тестами** - комплексні тести з pytest
- 🔄 **Міграції БД** - Alembic для управління схемою бази даних
- 🚦 **CORS підтримка** - готовність до фронтенд інтеграції

## 🛠 Технологічний стек

| Компонент | Технологія | Версія |
|-----------|------------|--------|
| **Web Framework** | FastAPI | 0.104+ |
| **ASGI Server** | Uvicorn | 0.24+ |
| **ORM** | SQLAlchemy | 2.0+ |
| **База даних** | PostgreSQL | 15+ |
| **Валідація** | Pydantic | 2.5+ |
| **Міграції** | Alembic | 1.12+ |
| **Контейнеризація** | Docker & Docker Compose | Latest |
| **Управління залежностями** | Poetry | 1.6+ |
| **Тестування** | pytest + httpx | Latest |
| **Тестові дані** | Faker | 20.1+ |

## 📁 Структура проекту

```
contacts-api/
├── 📂 app/                          # Основний код додатку
│   ├── __init__.py                 # Ініціалізація пакету
│   ├── main.py                     # Головний FastAPI додаток
│   ├── database.py                 # Налаштування SQLAlchemy
│   ├── models.py                   # Моделі бази даних
│   ├── schemas.py                  # Pydantic схеми валідації
│   ├── crud.py                     # CRUD операції
│   └── 📂 routers/                 # API роутери
│       ├── __init__.py
│       └── contacts.py             # Endpoints для контактів
├── 📂 tests/                       # Тестування
│   ├── __init__.py
│   ├── conftest.py                 # Налаштування pytest
│   └── test_contacts.py            # Тести API контактів
├── 📂 scripts/                     # Допоміжні скрипти
│   └── seed_data.py                # Генерація тестових даних
├── 📂 alembic/                     # Міграції бази даних
│   ├── versions/                   # Файли міграцій
│   ├── env.py                      # Налаштування Alembic
│   └── script.py.mako              # Шаблон міграцій
├── 🐳 docker-compose.yml           # Docker Compose конфігурація
├── 🐳 Dockerfile                   # Docker образ
├── 📦 pyproject.toml               # Poetry конфігурація
├── ⚙️ alembic.ini                  # Конфігурація міграцій
├── 🔒 .env.example                 # Приклад змінних середовища
└── 📖 README.md                    # Ця документація
```

## ⚡ Швидкий старт

### 🐳 Метод 1: Docker (Рекомендовано)

```bash
# 1. Клонувати репозиторій
git clone https://github.com/sandychips/goit-pythonweb-hw-08.git
cd goit-pythonweb-hw-08

# 2. Налаштувати змінні середовища
cp .env.example .env
# Відредагуйте .env файл за потребою

# 3. Запустити всі сервіси
docker-compose up -d

# 4. Дочекатися готовності сервісів (10-15 секунд)
docker-compose logs -f web

# 5. Створити тестові дані
docker-compose exec web python scripts/seed_data.py

# 6. Перевірити роботу API
curl http://localhost:8000/health
```

**🎉 Готово!** API доступне за адресою: **http://localhost:8000/docs**

### 📦 Метод 2: Локальний розвиток з Poetry

```bash
# 1. Встановити Poetry (якщо не встановлено)
curl -sSL https://install.python-poetry.org | python3 -

# 2. Клонувати та встановити залежності
git clone https://github.com/sandychips/goit-pythonweb-hw-08.git
cd goit-pythonweb-hw-08
poetry install

# 3. Налаштувати змінні середовища
cp .env.example .env

# 4. Запустити PostgreSQL
docker run --name postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 -d postgres:15

# 5. Активувати віртуальне середовище
poetry shell

# 6. Створити тестові дані
python scripts/seed_data.py

# 7. Запустити додаток
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 🐍 Метод 3: Стандартний pip

```bash
# 1. Створити віртуальне середовище
python -m venv venv
source venv/bin/activate  # Linux/Mac
# або venv\Scripts\activate  # Windows

# 2. Встановити залежності
pip install -r requirements.txt

# 3. Налаштувати змінні середовища
cp .env.example .env

# 4. Запустити PostgreSQL та додаток
# (аналогічно до методу з Poetry)
```

## 📖 API Документація

Після запуску додатку документація автоматично доступна за адресами:

| Тип документації | URL | Опис |
|------------------|-----|------|
| **Swagger UI** | http://localhost:8000/docs | Інтерактивна документація з можливістю тестування |
| **ReDoc** | http://localhost:8000/redoc | Альтернативна документація у стилі ReDoc |
| **OpenAPI JSON** | http://localhost:8000/openapi.json | Схема API у форматі OpenAPI 3.0 |

## 🔗 API Endpoints

### 📋 Основні операції з контактами

| HTTP Метод | Endpoint | Опис | Параметри |
|------------|----------|------|-----------|
| `GET` | `/api/v1/contacts/` | Отримати список контактів | `skip`, `limit`, `search` |
| `POST` | `/api/v1/contacts/` | Створити новий контакт | JSON body |
| `GET` | `/api/v1/contacts/{id}` | Отримати контакт за ID | `id` в URL |
| `PUT` | `/api/v1/contacts/{id}` | Оновити контакт | `id` в URL + JSON body |
| `DELETE` | `/api/v1/contacts/{id}` | Видалити контакт | `id` в URL |

### 🎂 Спеціальні endpoints

| HTTP Метод | Endpoint | Опис |
|------------|----------|------|
| `GET` | `/api/v1/contacts/birthdays` | Контакти з ДН на найближчі 7 днів |

### 🔍 Query параметри для пошуку та пагінації

- **`search`** - пошук за ім'ям, прізвищем або email (регістронезалежний)
- **`skip`** - кількість записів для пропуску (за замовчуванням: 0)
- **`limit`** - максимальна кількість записів (за замовчуванням: 100, макс: 1000)

## 💡 Приклади використання

### 📝 Створення нового контакту

```bash
curl -X POST "http://localhost:8000/api/v1/contacts/" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Олександр",
    "last_name": "Шевченко",
    "email": "oleksandr.shevchenko@example.com",
    "phone": "+380501234567",
    "birthday": "1990-03-15",
    "additional_info": "Колега з роботи, експерт з Python"
  }'
```

**Відповідь:**
```json
{
  "id": 1,
  "first_name": "Олександр",
  "last_name": "Шевченко",
  "email": "oleksandr.shevchenko@example.com",
  "phone": "+380501234567",
  "birthday": "1990-03-15",
  "additional_info": "Колега з роботи, експерт з Python"
}
```

### 📋 Отримання списку контактів

```bash
# Всі контакти
curl "http://localhost:8000/api/v1/contacts/"

# З пагінацією
curl "http://localhost:8000/api/v1/contacts/?skip=10&limit=5"

# Пошук за ім'ям
curl "http://localhost:8000/api/v1/contacts/?search=Олександр"

# Пошук за email
curl "http://localhost:8000/api/v1/contacts/?search=@example.com"
```

### 🎂 Найближчі дні народження

```bash
curl "http://localhost:8000/api/v1/contacts/birthdays"
```

### ✏️ Оновлення контакту

```bash
curl -X PUT "http://localhost:8000/api/v1/contacts/1" \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "+380507654321",
    "additional_info": "Оновлена інформація про контакт"
  }'
```

### 🗑️ Видалення контакту

```bash
curl -X DELETE "http://localhost:8000/api/v1/contacts/1"
```

## 🗃 Структура даних

### 📋 Модель контакту

```python
{
  "id": 1,                                    # Унікальний ідентифікатор
  "first_name": "Олександр",                  # Ім'я (1-50 символів)
  "last_name": "Шевченко",                   # Прізвище (1-50 символів)
  "email": "oleksandr@example.com",          # Email (унікальний)
  "phone": "+380501234567",                  # Телефон (10-20 символів)
  "birthday": "1990-03-15",                  # Дата народження (YYYY-MM-DD)
  "additional_info": "Додаткова інформація"  # Опціональне поле (до 500 символів)
}
```

### ✅ Правила валідації

- **`first_name`** - обов'язкове, 1-50 символів
- **`last_name`** - обов'язкове, 1-50 символів  
- **`email`** - обов'язкове, валідний email, унікальний в системі
- **`phone`** - обов'язкове, 10-20 символів
- **`birthday`** - обов'язкове, валідна дата у форматі ISO (YYYY-MM-DD)
- **`additional_info`** - опціональне, до 500 символів

## ⚙️ Налаштування

### 🔧 Змінні середовища (.env)

```env
# 🗄️ База даних
DATABASE_URL=postgresql://postgres:password@localhost:5432/contacts_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
POSTGRES_DB=contacts_db

# 🚀 Додаток
DEBUG=True
HOST=0.0.0.0
PORT=8000
```

### 🐳 Docker сервіси

| Сервіс | Порт | Опис |
|--------|------|------|
| **web** | 8000 | FastAPI додаток |
| **db** | 5432 | PostgreSQL база даних |
| **adminer** | 8080 | Веб-інтерфейс для управління БД |

## 🔄 Міграції бази даних

### Основні команди Alembic

```bash
# Створити нову міграцію (автоматично)
alembic revision --autogenerate -m "Опис змін"

# Застосувати всі міграції
alembic upgrade head

# Відкотити до попередньої версії
alembic downgrade -1

# Переглянути історію міграцій
alembic history

# Поточна версія схеми
alembic current
```

## 🧪 Тестування

### Запуск тестів

```bash
# Всі тести
poetry run pytest

# Тести з детальним виводом
poetry run pytest -v

# Тести з покриттям коду
poetry run pytest --cov=app --cov-report=html

# Конкретний тест
poetry run pytest tests/test_contacts.py::test_create_contact
```

### Структура тестів

- **test_create_contact** - тестування створення контактів
- **test_get_contacts** - тестування отримання списку
- **test_search_contacts** - тестування функції пошуку
- **test_upcoming_birthdays** - тестування днів народження
- **test_crud_operations** - тестування всіх CRUD операцій
- **test_validation_errors** - тестування валідації даних

## 🎭 Тестові дані

### Генерація реалістичних даних

```bash
# Створити 20 тестових контактів (за замовчуванням)
python scripts/seed_data.py

# Через Docker
docker-compose exec web python scripts/seed_data.py
```

**Особливості тестових даних:**
- 📍 Українська локалізація (імена, прізвища)
- 🎂 Частина контактів матиме дні народження на найближчі 7 днів
- 📧 Унікальні email адреси
- 📞 Реалістичні номери телефонів
- 📝 Випадкова додаткова інформація

## 🐳 Docker команди

### Управління сервісами

```bash
# Запустити всі сервіси
docker-compose up -d

# Зупинити всі сервіси
docker-compose down

# Перезапустити конкретний сервіс
docker-compose restart web

# Переглянути логи
docker-compose logs -f web

# Підключитися до контейнера
docker-compose exec web bash

# Переглянути статус сервісів
docker-compose ps
```

### Управління даними

```bash
# Створити резервну копію БД
docker-compose exec db pg_dump -U postgres contacts_db > backup.sql

# Відновити з резервної копії
docker-compose exec -T db psql -U postgres contacts_db < backup.sql

# Очистити дані БД
docker-compose down -v
```

## 🚀 Розгортання

### 🌐 Продакшен налаштування

1. **Безпека:**
   - Використовуйте сильні паролі для бази даних
   - Налаштуйте HTTPS
   - Обмежте CORS origins
   - Використовуйте змінні середовища для секретів

2. **База даних:**
   - Налаштуйте connection pooling
   - Регулярні backup'и
   - Моніторинг продуктивності

3. **Масштабування:**
   - Reverse proxy (nginx/traefik)
   - Load balancing
   - Horizontal scaling з Kubernetes

### 🔧 Приклад продакшен конфігурації

```yaml
# docker-compose.prod.yml
version: '3.8'
services:
  web:
    build: .
    environment:
      - DEBUG=False
      - DATABASE_URL=postgresql://user:pass@prod-db:5432/contacts
    restart: unless-stopped
    deploy:
      replicas: 3
  
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - web
```

## 🤝 Внесок у проект

Ми вітаємо внески в розвиток проекту! Ось як ви можете допомогти:

1. **🐛 Повідомлення про баги** - створіть Issue з детальним описом
2. **✨ Нові функції** - спочатку обговоріть в Issues
3. **📖 Покращення документації** - завжди вітається
4. **🧪 Більше тестів** - допоможіть покращити покриття

### Процес контрибуції

```bash
# 1. Форк репозиторію
git clone https://github.com/YOUR_USERNAME/goit-pythonweb-hw-08.git

# 2. Створіть нову гілку
git checkout -b feature/amazing-feature

# 3. Зробіть зміни та тести
# ... ваші зміни ...

# 4. Запустіть тести
pytest

# 5. Зробіть коміт
git commit -m "Add amazing feature"

# 6. Надішліть зміни
git push origin feature/amazing-feature

# 7. Створіть Pull Request
```

## 📊 Статус проекту

| Компонент | Статус | Покриття |
|-----------|--------|----------|
| ✅ **CRUD операції** | Готово | 100% |
| ✅ **Пошук та фільтрація** | Готово | 100% |
| ✅ **Валідація даних** | Готово | 100% |
| ✅ **API документація** | Готово | 100% |
| ✅ **Docker підтримка** | Готово | 100% |
| ✅ **Тестування** | Готово | 95%+ |
| ⏳ **Автентифікація** | Планується | - |
| ⏳ **Rate limiting** | Планується | - |
| ⏳ **Кешування** | Планується | - |

## 🆘 Підтримка та допомога

### 🐛 Проблеми та баги

Якщо ви зіткнулися з проблемами:

1. **Перевірте Issues** - можливо проблема вже відома
2. **Створіть новий Issue** з детальним описом:
   - Кроки для відтворення
   - Очікувана поведінка
   - Фактична поведінка
   - Версія Python, ОС, Docker
   - Логи помилок

### 💬 Обговорення

- **GitHub Discussions** - для загальних питань
- **Issues** - для багів та фіч
- **Pull Requests** - для контрибуцій

### 📚 Додаткові ресурси

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Poetry Documentation](https://python-poetry.org/docs/)

## 📄 Ліцензія

Цей проект поширюється під ліцензією **MIT License**. Дивіться файл [LICENSE](LICENSE) для детальної інформації.

```
MIT License

Copyright (c) 2024 Contacts API Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 📈 Дорожна карта

### Версія 1.1 (Наступна)
- 🔐 JWT автентифікація та авторизація
- 🚦 Rate limiting для API endpoints
- 📧 Email нагадування про дні народження
- 🔍 Розширений пошук з фільтрами

### Версія 1.2 (Майбутня)
- 💾 Redis кешування
- 📊 Метрики та моніторинг
- 🌍 Інтернаціоналізація (i18n)
- 📱 GraphQL endpoints

### Версія 2.0 (Довгострокова)
- 🏢 Мультитенантність
- 🔄 Real-time оновлення (WebSockets)
- 📸 Підтримка аватарів контактів
- 🏷️ Система тегів та категорій

---


**FastAPI** • **SQLAlchemy** • **PostgreSQL** • **Docker** • **Python 3.11+**

