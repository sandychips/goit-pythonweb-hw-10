# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# CORS
ALLOWED_ORIGINS=http://localhost:3000,https://yourdomain.com

# App
DEBUG=True
HOST=0.0.0.0
PORT=8000
```

## 🚀 Workflow розробки

### 1. 📋 Етапи створення проекту

#### **Фаза 1: Базова структура**
```bash
# Створення директорій
mkdir -p contacts-api/{app/routers,tests,scripts,alembic}

# Poetry ініціалізація
cd contacts-api && poetry init

# Базові файли
touch app/{__init__.py,main.py,database.py,models.py}
```

#### **Фаза 2: База даних та моделі**
```bash
# SQLAlchemy setup
app/database.py    # Підключення
app/models.py      # User, Contact, EmailVerification
app/schemas.py     # Pydantic валідація

# Alembic міграції
alembic init alembic
alembic revision --autogenerate -m "Initial tables"
```

#### **Фаза 3: Аутентифікація**
```bash
# JWT та безпека
app/auth.py        # Токени, хешування
app/routers/auth.py # Register, login endpoints
app/email.py       # Верифікація email

# Rate limiting
app/rate_limiter.py # SlowAPI налаштування
```

#### **Фаза 4: Бізнес логіка**
```bash
# CRUD операції
app/crud.py        # DB операції з користувачами
app/routers/contacts.py # API endpoints

# Зовнішні сервіси
app/cloudinary_service.py # Завантаження файлів
```

#### **Фаза 5: Тестування**
```bash
# Тестова інфраструктура
tests/conftest.py     # Фікстури
tests/test_auth.py    # Аутентифікація
tests/test_contacts.py # CRUD тести

# Генерація даних
scripts/seed_data.py  # Faker тестові дані
```

### 2. 🔄 Development workflow

#### **Локальна розробка**
```bash
# 1. Підготовка середовища
poetry install
cp .env.example .env

# 2. База даних
docker-compose up -d db redis

# 3. Міграції та дані
poetry shell
alembic upgrade head
python scripts/seed_data.py

# 4. Запуск з hot reload
uvicorn app.main:app --reload
```

#### **Тестування**
```bash
# Unit тести
pytest tests/ -v

# Покриття коду
pytest --cov=app --cov-report=html

# Окремі модулі
pytest tests/test_auth.py::test_user_registration -v
```

#### **Docker розробка**
```bash
# Повний стек
docker-compose up -d

# Тільки залежності
docker-compose up -d db redis adminer

# Логи та дебаг
docker-compose logs -f web
```

## 📊 Ключові особливості архітектури

### 🔐 **Багаторівнева аутентифікація**

```python
# 1. Реєстрація → Неверифікований користувач
# 2. Email verification → Верифікований користувач  
# 3. JWT токен → Доступ до API
# 4. Rate limiting → Захист від атак

Dependencies hierarchy:
get_current_user() → get_current_verified_user() → API endpoints
```

### 👥 **Мультитенантність на рівні користувачів**

```python
# Кожен користувач бачить тільки свої контакти
def get_user_contacts(db: Session, user_id: int):
    return db.query(Contact).filter(Contact.owner_id == user_id)

# Foreign key constraint забезпечує data isolation
class Contact(Base):
    owner_id = Column(Integer, ForeignKey("users.id"), nullable=False)
```

### 📧 **Асинхронна email система**

```python
# FastAPI-Mail з HTML шаблонами
async def send_verification_email(email: str, token: str):
    verification_url = f"{base_url}/verify-email?token={token}"
    # HTML template з кнопкою верифікації
```

### ☁️ **Cloud-ready файлова система**

```python
# Cloudinary інтеграція
async def upload_avatar(file: UploadFile, user_id: int):
    # Автоматичне стиснення до 300x300
    # Унікальні public_id для користувачів
    # Error handling та валідація
```

## 🛡️ Безпека та Compliance

### **Аутентифікація**
- ✅ JWT токени з коротким TTL (30 хв)
- ✅ Bcrypt хешування паролів (cost 12)
- ✅ Email верифікація обов'язкова
- ✅ Rate limiting на критичних endpoints

### **Авторизація**  
- ✅ Resource-level isolation (owner_id)
- ✅ Middleware перевірка токенів
- ✅ Dependency injection для permissions

### **Валідація даних**
- ✅ Pydantic схеми для всіх inputs
- ✅ SQL injection захист (SQLAlchemy ORM)
- ✅ File upload валідація (розмір, тип)

### **CORS та Headers**
- ✅ Configurable origins
- ✅ Secure headers middleware
- ✅ HTTPS ready конфігурація

## 📈 Масштабування та Performance

### **База даних**
```python
# Індекси для швидкого пошуку
class Contact(Base):
    email = Column(String(100), nullable=False, index=True)
    first_name = Column(String(50), nullable=False, index=True)
    
# Пагінація для великих списків
def get_contacts(skip: int = 0, limit: int = 100):
```

### **Кешування**
```python
# Redis для rate limiting
# Session-based кешування JWT verification
# Cloudinary CDN для статичних файлів
```

### **Горизонтальне масштабування**
```yaml
# Stateless додаток - ready для:
- Kubernetes deployment
- Load balancer розподіл
- Multiple instances
- Database connection pooling
```

## 🔍 Моніторинг та Logging

### **Структуроване логування**
```python
# FastAPI автоматичні логи:
- HTTP requests/responses
- JWT token validation
- Database query errors
- Rate limiting violations
```

### **Health checks**
```python
# Multiple endpoints:
GET /health        # App status
GET /api/v1/auth/me # Auth status (rate limited)
```

### **Метрики**
```python
# Ready для інтеграції:
- Prometheus metrics
- Performance monitoring
- Error tracking (Sentry)
- Database performance
```

## 🎯 Production Checklist

### **Безпека**
- [ ] Змінити SECRET_KEY на cryptographically secure
- [ ] Налаштувати HTTPS/TLS
- [ ] Обмежити CORS origins
- [ ] Використати керовані секрети (AWS/Azure/GCP)

### **База даних**
- [ ] Керований PostgreSQL сервіс
- [ ] Connection pooling
- [ ] Регулярні backup'и
- [ ] Моніторинг performance

### **Інфраструктура**
- [ ] Container orchestration (Kubernetes)
- [ ] Load balancer + Auto-scaling
- [ ] CDN для статичних файлів
- [ ] Централізоване логування

### **Моніторинг**
- [ ] APM система (DataDog/New Relic)
- [ ] Error tracking (Sentry)
- [ ] Uptime monitoring
- [ ] Performance alerts

## 📚 Розширення функціоналу

### **Можливі додавання**
```python
# User features:
- Password reset functionality
- Two-factor authentication (2FA)
- Social login (OAuth2)
- User roles and permissions

# Contact features:
- Contact categories/tags
- Import/Export (CSV, vCard)
- Contact sharing between users
- Advanced search filters

# Technical:
- GraphQL API endpoint
- WebSocket real-time updates
- Background tasks (Celery)
- API versioning strategy
```

### **Архітектурні поліпшення**
```python
# Patterns to implement:
- Repository pattern for data access
- Event-driven architecture
- CQRS for read/write separation
- Domain-driven design structure
```

## 🎓 Навчальні аспекти

### **Демонстровані концепції**
- ✅ Modern Python development (Poetry, type hints)
- ✅ FastAPI best practices
- ✅ SQLAlchemy 2.0 patterns
- ✅ JWT authentication flow
- ✅ Docker containerization
- ✅ Comprehensive testing
- ✅ Cloud services integration
- ✅ Security considerations

### **Production-ready features**
- ✅ Environment-based configuration
- ✅ Database migrations
- ✅ Error handling and validation
- ✅ API documentation
- ✅ Performance optimization
- ✅ Monitoring capabilities

Цей проект демонструє повний lifecycle сучасного Python API - від простого CRUD до enterprise-ready системи з аутентифікацією, тестуванням та cloud інтеграцією. 🚀 📁 Повна структура проекту Contacts API з аутентифікацією

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
