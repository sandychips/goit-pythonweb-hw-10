# 📁 Повна структура проекту Contacts API з аутентифікацією

## 🎯 Огляд проекту

Contacts API еволюціонував від простого CRUD додатку до повноцінної системи з аутентифікацією. Проект демонструє сучасні практики розробки API з використанням FastAPI, JWT токенів, верифікації email та cloud сервісів.

## 📂 Структура директорій

```
contacts-api/
├── 📁 app/                           # 🏗️ Основний код додатку
│   ├── 📄 __init__.py               # Ініціалізація пакету
│   ├── 🚀 main.py                   # FastAPI додаток + middleware
│   ├── 🗄️ database.py               # SQLAlchemy конфігурація
│   ├── 📊 models.py                 # User, Contact, EmailVerification
│   ├── ✅ schemas.py                # Pydantic схеми валідації
│   ├── 💾 crud.py                   # CRUD з підтримкою користувачів
│   ├── 🔐 auth.py                   # JWT токени + авторизація
│   ├── ✉️ email.py                   # FastAPI-Mail сервіс
│   ├── ☁️ cloudinary_service.py      # Завантаження аватарів
│   ├── 🛡️ rate_limiter.py           # SlowAPI rate limiting
│   └── 📁 routers/                  # API роутери
│       ├── 📄 __init__.py
│       ├── 👤 auth.py               # Реєстрація/логін/профіль
│       └── 📞 contacts.py           # CRUD операції контактів
├── 📁 tests/                        # 🧪 Тестове покриття
│   ├── 📄 __init__.py
│   ├── ⚙️ conftest.py               # Pytest фікстури + тестовий DB
│   ├── 🔒 test_auth.py              # Тести аутентифікації
│   └── 📞 test_contacts.py          # Тести API контактів
├── 📁 scripts/                      # 🛠️ Допоміжні утиліти
│   └── 🎭 seed_data.py              # Faker генерація даних
├── 📁 alembic/                      # 📈 Міграції бази даних
│   ├── 📁 versions/                 # Історія змін схеми
│   ├── ⚙️ env.py                    # Alembic конфігурація
│   ├── 📝 script_mako.py            # Шаблон міграцій
│   └── 📋 readme.md                 # Документація Alembic
├── 📁 app/templates/                # 📧 Email шаблони
│   └── 📄 .gitkeep                  # Збереження папки
├── 🔧 .env.example                  # Шаблон змінних середовища
├── 🚫 .gitignore                    # Git ignore правила
├── 🐳 Dockerfile                    # Docker конфігурація
├── 🐙 docker-compose.yml            # Оркестрація сервісів
├── ⚙️ alembic.ini                   # Alembic налаштування
├── 📦 pyproject.toml                # Poetry залежності
├── 🔒 poetry.lock                   # Версії пакетів
├── 🛠️ makefile.txt                  # Makefile команди
└── 📖 README.md                     # Документація проекту
```

## 🏗️ Архітектурний огляд

### Рівень додатку (`/app`)

#### 🚀 **main.py** - Точка входу
```python
# Основні компоненти:
- FastAPI додаток з middleware
- CORS налаштування
- Rate limiting setup
- Роутери: auth + contacts
- Health check endpoints
```

#### 🗄️ **database.py** - Підключення до БД
```python
# SQLAlchemy конфігурація:
- Підключення до PostgreSQL/SQLite
- Session factory
- get_db() dependency
- Environment-based конфігурація
```

#### 📊 **models.py** - Моделі даних
```python
# SQLAlchemy моделі:
- User (з is_verified, avatar_url)
- Contact (з owner_id foreign key)
- EmailVerification (токени верифікації)
- Relationships та constraints
```

#### ✅ **schemas.py** - Валідація
```python
# Pydantic схеми:
- User: Create, Update, Response, Login
- Contact: Create, Update, Response
- Token: JWT, TokenData
- Email: VerificationRequest
```

#### 💾 **crud.py** - Операції з БД
```python
# Функції:
- User CRUD: create, authenticate, update
- Contact CRUD: з owner_id фільтрацією
- Email verification: create/verify tokens
- Upcoming birthdays з SQL фільтрами
```

#### 🔐 **auth.py** - Безпека
```python
# JWT аутентифікація:
- Password hashing (bcrypt)
- JWT токени (python-jose)
- Dependencies: get_current_user
- Token verification та генерація
```

### API Роутери (`/app/routers`)

#### 👤 **auth.py** - Аутентифікація
```python
# Endpoints:
POST /register     - реєстрація + email відправка
POST /login        - JWT токен отримання
GET  /verify-email - верифікація через токен
POST /resend-verification - повторна відправка
GET  /me          - профіль користувача (rate limited)
PUT  /me          - оновлення профілю
POST /me/avatar   - завантаження на Cloudinary
```

#### 📞 **contacts.py** - Контакти
```python
# Endpoints (всі з JWT required):
POST   /          - створення контакту
GET    /          - список з пошуком/пагінацією
GET    /{id}      - отримання за ID
PUT    /{id}      - оновлення
DELETE /{id}      - видалення
GET    /birthdays - найближчі 7 днів
```

### Зовнішні сервіси

#### ✉️ **email.py** - Відправка листів
```python
# FastAPI-Mail конфігурація:
- SMTP налаштування (Gmail)
- HTML шаблони
- Верифікаційні посилання
- Async відправка
```

#### ☁️ **cloudinary_service.py** - Файли
```python
# Cloudinary інтеграція:
- Завантаження аватарів
- Автоматичне стиснення (300x300)
- Валідація формату/розміру
- Error handling
```

#### 🛡️ **rate_limiter.py** - Обмеження
```python
# SlowAPI налаштування:
- Redis/Memory backend
- Per-endpoint ліміти
- IP-based обмеження
- Graceful fallback
```

## 🧪 Тестова структура (`/tests`)

### ⚙️ **conftest.py** - Налаштування
```python
# Pytest фікстури:
- In-memory SQLite для тестів
- TestClient з dependency override
- Authenticated client фікстура
- Sample data fixtures
```

### 🔒 **test_auth.py** - Тести аутентифікації
```python
# Сценарії:
- Реєстрація (успіх/дублікат)
- Логін (успіх/невірні дані)
- Верифікація email
- Профіль (отримання/оновлення)
- JWT токени (валідні/невалідні)
- Unauthorized доступ
```

### 📞 **test_contacts.py** - Тести контактів
```python
# Сценарії:
- CRUD операції з аутентифікацією
- Пошук та пагінація
- Дні народження функціонал
- Валідаційні помилки
- Isolation між користувачами
```

## 🐳 Docker інфраструктура

### **docker-compose.yml** - Сервіси
```yaml
services:
  web:        # FastAPI + dependencies
  db:         # PostgreSQL 15
  redis:      # Redis для rate limiting
  adminer:    # DB веб-інтерфейс
```

### **Dockerfile** - Multi-stage build
```dockerfile
# Production-ready образ:
- Python 3.11 slim base
- Poetry для залежностей
- Non-root user
- Health checks
```

## 📦 Управління залежностями

### **pyproject.toml** - Poetry конфігурація
```toml
[tool.poetry.dependencies]
# Core
fastapi = "^0.104.1"
uvicorn = {extras = ["standard"], version = "^0.24.0"}
sqlalchemy = "^2.0.23"
pydantic = {extras = ["email"], version = "^2.5.0"}

# Auth & Security  
python-jose = {extras = ["cryptography"], version = "^3.3.0"}
passlib = {extras = ["bcrypt"], version = "^1.7.4"}
slowapi = "^0.1.9"

# External Services
cloudinary = "^1.36.0"
fastapi-mail = "^1.4.1" 
redis = "^5.0.1"

# Development
[tool.poetry.group.dev.dependencies]
pytest = "^7.4.3"
pytest-asyncio = "^0.21.1"
httpx = "^0.25.2"
```

## 🔧 Налаштування середовища

### **.env.example** - Шаблон конфігурації
```env
# Database
DATABASE_URL=postgresql://user:pass@host:5432/db

# JWT
SECRET_KEY=secure-key-for-production
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Email (Gmail)
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=app-specific-password

# Cloudinary
CLOUDINARY_CLOUD_NAME=your-cloud
CLOUDINARY_API_KEY=your-key
CLOUDINARY_API_SECRET=your-secret

#
