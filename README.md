# 🔐 Contacts REST API з аутентифікацією

Повнофункціональний REST API для управління контактами з JWT аутентифікацією, верифікацією email, завантаженням аватарів та rate limiting.

## 🚀 Ключові особливості

### Основний функціонал
- ✅ **CRUD операції** - повний набір операцій для управління контактами
- 🔍 **Пошук контактів** - за ім'ям, прізвищем або email
- 🎂 **Дні народження** - отримання контактів з ДН на найближчі 7 днів
- 📱 **Персональні контакти** - кожен користувач бачить тільки свої контакти

### Система аутентифікації
- 🔐 **JWT токени** - безпечна аутентифікація з Bearer токенами
- ✉️ **Верифікація email** - підтвердження реєстрації через email
- 👤 **Управління профілем** - оновлення інформації користувача
- 📸 **Завантаження аватарів** - інтеграція з Cloudinary
- 🔒 **Захист від атак** - rate limiting на критичні endpoints

### Технічні можливості
- ✨ **Валідація даних** - використання Pydantic для перевірки вхідних даних
- 📚 **Автодокументація** - Swagger UI та ReDoc
- 🐳 **Docker підтримка** - повна контейнеризація з Docker Compose
- 🎭 **Тестові дані** - генерація реалістичних даних з Faker
- 🧪 **Тестування** - повне покриття pytest тестами
- 📧 **Email сервіс** - відправка листів через FastAPI-Mail

## 🛠 Технологічний стек

### Backend Framework
- **FastAPI 0.104+** - сучасний async веб-фреймворк для Python
- **SQLAlchemy 2.0+** - ORM з підтримкою async операцій
- **PostgreSQL 15** - надійна реляційна база даних
- **Pydantic 2.0+** - валідація та серіалізація даних з type hints
- **Alembic** - версіонування та міграції схеми БД

### Аутентифікація та безпека
- **python-jose[cryptography]** - JWT токени з RSA/ECDSA шифруванням
- **passlib[bcrypt]** - безпечне хешування паролів з adaptive cost
- **slowapi + Redis** - distributed rate limiting з персистентністю
- **python-multipart** - обробка multipart/form-data для файлів

### Зовнішні сервіси та інтеграції
- **Cloudinary** - cloud-based зберігання, оптимізація та CDN для зображень
- **FastAPI-Mail** - async відправка email з SMTP підтримкою
- **Redis 7** - in-memory кешування та session store

### DevOps та тестування
- **Poetry** - modern dependency management з lock файлами
- **pytest + pytest-asyncio** - комплексне тестування з async підтримкою
- **httpx** - async HTTP клієнт для тестування API
- **Faker** - генерація реалістичних тестових даних
- **Docker + Docker Compose** - контейнеризація та оркестрація

## 📁 Структура проекту

```
contacts-api/
├── app/                          # 🏗️ Основний код додатку
│   ├── main.py                   # FastAPI додаток з middleware та роутерами
│   ├── database.py               # SQLAlchemy engine та session management
│   ├── models.py                 # Моделі: User, Contact, EmailVerification
│   ├── schemas.py                # Pydantic схеми для request/response валідації
│   ├── crud.py                   # Database access layer з user isolation
│   ├── auth.py                   # JWT токени, dependencies та авторизація
│   ├── email.py                  # Async email service з HTML templates
│   ├── cloudinary_service.py     # File upload та image processing
│   ├── rate_limiter.py           # SlowAPI конфігурація та middleware
│   └── routers/                  # 📡 API endpoint роутери
│       ├── auth.py               # Authentication: register, login, profile
│       └── contacts.py           # Contact management CRUD operations
├── tests/                        # 🧪 Comprehensive test suite
│   ├── conftest.py               # Pytest fixtures, test database setup
│   ├── test_auth.py              # Authentication flow testing
│   └── test_contacts.py          # Contact API endpoint testing
├── scripts/                      # 🛠️ Utility scripts
│   └── seed_data.py              # Test data generation with Faker
├── alembic/                      # 📈 Database migrations
│   ├── versions/                 # Migration history files
│   ├── env.py                    # Alembic environment configuration
│   └── script.py.mako            # Migration template
├── 🔧 .env.example               # Environment variables template
├── 🐳 docker-compose.yml         # Multi-service orchestration
├── 📦 pyproject.toml             # Poetry dependencies and project config
└── 📖 README.md                  # Project documentation
```

## 📋 Системні вимоги

### Мінімальні вимоги
- **Python 3.11+** (рекомендовано 3.12)
- **PostgreSQL 12+** (рекомендовано 15)
- **Redis 6+** (рекомендовано 7)
- **4GB RAM** (мінімум для development)
- **Docker 20+ та Docker Compose 2+** (для контейнерного запуску)

### Рекомендовані налаштування
- **8GB RAM** для комфортної розробки
- **SSD диск** для швидкості PostgreSQL
- **Poetry 1.6+** для управління залежностями

## ⚡ Покрокова установка та запуск

### 🚀 Метод 1: Docker Compose (Найпростіший - рекомендовано для початківців)

#### Крок 1: Підготовка середовища
```bash
# Клонувати репозиторій
git clone https://github.com/your-username/contacts-api.git
cd contacts-api

# Перевірити наявність Docker
docker --version
docker-compose --version

# Якщо Docker не встановлений:
# Linux (Ubuntu/Debian): sudo apt update && sudo apt install docker.io docker-compose
# macOS: brew install docker docker-compose
# Windows: Завантажити Docker Desktop з офіційного сайту
```

#### Крок 2: Конфігурація змінних середовища
```bash
# Скопіювати шаблон конфігурації
cp .env.example .env

# Відредагувати .env файл (ОБОВ'ЯЗКОВО!)
nano .env  # або code .env для VS Code

# Мінімальні зміни для локального запуску:
SECRET_KEY=your-super-secret-key-minimum-32-characters-long-please-change-this
MAIL_USERNAME=your-gmail@gmail.com
MAIL_PASSWORD=your-app-specific-password
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret
```

#### Крок 3: Запуск всього стеку
```bash
# Запустити всі сервіси у фоновому режимі
docker-compose up -d

# Перевірити статус сервісів
docker-compose ps

# Переглянути логи (опціонально)
docker-compose logs -f web
```

#### Крок 4: Ініціалізація даних
```bash
# Створити тестові дані
docker-compose exec web python scripts/seed_data.py

# Перевірити роботу API
curl http://localhost:8000/health
```

#### Крок 5: Тестування інсталяції
```bash
# Відкрити автоматичну документацію
open http://localhost:8000/docs

# Або для Linux
xdg-open http://localhost:8000/docs

# Веб-інтерфейс бази даних (Adminer)
open http://localhost:8080
```

### 🛠️ Метод 2: Локальна розробка (Для досвідчених розробників)

#### Крок 1: Встановлення Poetry
```bash
# Встановити Poetry (якщо не встановлений)
curl -sSL https://install.python-poetry.org | python3 -

# Або через pip
pip install poetry

# Перевірити встановлення
poetry --version

# Налаштувати Poetry (створювати .venv в проекті)
poetry config virtualenvs.in-project true
```

#### Крок 2: Встановлення Python залежностей
```bash
# Клонувати проект
git clone https://github.com/your-username/contacts-api.git
cd contacts-api

# Встановити всі залежності (включно з dev)
poetry install

# Переглянути встановлені пакети
poetry show

# Активувати віртуальне середовище
poetry shell
```

#### Крок 3: Налаштування зовнішніх сервісів
```bash
# Запустити тільки PostgreSQL та Redis через Docker
docker-compose up -d db redis

# Або встановити локально:
# PostgreSQL (Ubuntu): sudo apt install postgresql postgresql-contrib
# Redis (Ubuntu): sudo apt install redis-server
# macOS: brew install postgresql redis
```

#### Крок 4: Конфігурація та міграції
```bash
# Скопіювати та налаштувати .env
cp .env.example .env
# Відредагувати DATABASE_URL для локального PostgreSQL

# Застосувати міграції бази даних
alembic upgrade head

# Перевірити структуру БД
alembic current
alembic history
```

#### Крок 5: Запуск development сервера
```bash
# Запустити з hot reload
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# Альтернативно через Poetry
poetry run uvicorn app.main:app --reload

# Для debug режиму
uvicorn app.main:app --reload --log-level debug
```

## 🔧 Детальне налаштування конфігурації

### Змінні середовища (.env файл)

```env
# === База даних ===
# PostgreSQL connection string
DATABASE_URL=postgresql://postgres:password@localhost:5432/contacts_db

# === JWT Аутентифікація ===
# КРИТИЧНО: Змініть це на продакшні!
SECRET_KEY=your-super-secret-jwt-key-minimum-32-characters-change-in-production
ACCESS_TOKEN_EXPIRE_MINUTES=30

# === Email налаштування (Gmail) ===
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-specific-password  # НЕ звичайний пароль!
MAIL_FROM=noreply@contacts-api.com
MAIL_FROM_NAME=Contacts API
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587

# === Cloudinary (для аватарів) ===
CLOUDINARY_CLOUD_NAME=your-cloudinary-cloud-name
CLOUDINARY_API_KEY=your-cloudinary-api-key
CLOUDINARY_API_SECRET=your-cloudinary-api-secret

# === Redis (для rate limiting) ===
REDIS_HOST=localhost
REDIS_PORT=6379

# === CORS налаштування ===
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:8080,https://yourdomain.com

# === Додаток ===
DEBUG=True                    # False для продакшн
HOST=0.0.0.0
PORT=8000
```

### 📧 Налаштування Gmail для email (ОБОВ'ЯЗКОВО)

1. **Увімкнути 2FA в Google аккаунті:**
   ```
   Google Account → Security → 2-Step Verification → Turn On
   ```

2. **Створити App Password:**
   ```
   Google Account → Security → App passwords → Select app: Mail
   → Select device: Other → Enter name: "Contacts API"
   → Copy the 16-character password
   ```

3. **Додати в .env файл:**
   ```env
   MAIL_USERNAME=your-real-email@gmail.com
   MAIL_PASSWORD=abcd-efgh-ijkl-mnop  # 16-character app password
   ```

### ☁️ Налаштування Cloudinary (для аватарів)

1. **Створити безкоштовний аккаунт:**
   ```
   https://cloudinary.com/users/register/free
   ```

2. **Отримати API ключі:**
   ```
   Dashboard → Account Details → API Keys
   ```

3. **Додати в .env:**
   ```env
   CLOUDINARY_CLOUD_NAME=your-cloud-name
   CLOUDINARY_API_KEY=123456789012345
   CLOUDINARY_API_SECRET=abcdefghijk-lmnopqrstuvwxyz123
   ```

## 🔐 Повний workflow аутентифікації (покроково)

### Крок 1: Реєстрація нового користувача

```bash
# Створити нового користувача
curl -X POST "http://localhost:8000/api/v1/auth/register" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "email": "john@example.com",
    "password": "securepassword123",
    "first_name": "John",
    "last_name": "Doe"
  }'
```

**Очікувана відповідь:**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "avatar_url": null,
  "is_active": true,
  "is_verified": false,  // Потребує верифікації email
  "created_at": "2024-01-15T10:30:00.123456"
}
```

### Крок 2: Верифікація email

```bash
# Користувач отримає лист з посиланням типу:
# http://localhost:8000/api/v1/auth/verify-email?token=abc123def456

# Для тестування можна верифікувати вручну:
curl "http://localhost:8000/api/v1/auth/verify-email?token=YOUR_TOKEN_FROM_EMAIL"

# Або повторно відправити лист верифікації:
curl -X POST "http://localhost:8000/api/v1/auth/resend-verification" \
  -H "Content-Type: application/json" \
  -d '{"email": "john@example.com"}'
```

### Крок 3: Авторизація та отримання JWT токена

```bash
# Увійти в систему
curl -X POST "http://localhost:8000/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "securepassword123"
  }'
```

**Відповідь:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJqb2huQGV4YW1wbGUuY29tIiwiZXhwIjoxNzA1MzE4MjAwfQ.signature",
  "token_type": "bearer"
}
```

### Крок 4: Використання JWT токена

```bash
# Зберегти токен у змінній
export JWT_TOKEN="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."

# Отримати інформацію про свій профіль
curl -X GET "http://localhost:8000/api/v1/auth/me" \
  -H "Authorization: Bearer $JWT_TOKEN"

# Оновити профіль
curl -X PUT "http://localhost:8000/api/v1/auth/me" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Johnathan",
    "last_name": "Doe-Smith"
  }'

# Завантажити аватар
curl -X POST "http://localhost:8000/api/v1/auth/me/avatar" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -F "file=@/path/to/your/avatar.jpg"
```

## 📞 Робота з контактами (покроково)

### Створення контактів

```bash
# Створити перший контакт
curl -X POST "http://localhost:8000/api/v1/contacts/" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Іван",
    "last_name": "Петренко",
    "email": "ivan.petrenko@example.com",
    "phone": "+380501234567",
    "birthday": "1990-05-15",
    "additional_info": "Колега з роботи"
  }'

# Створити контакт з найближчим днем народження
curl -X POST "http://localhost:8000/api/v1/contacts/" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Марія",
    "last_name": "Іваненко",
    "email": "maria@example.com",
    "phone": "+380507654321",
    "birthday": "1995-12-25"
  }'
```

### Пошук та фільтрація

```bash
# Отримати всі контакти
curl -H "Authorization: Bearer $JWT_TOKEN" \
  "http://localhost:8000/api/v1/contacts/"

# Пошук за ім'ям
curl -H "Authorization: Bearer $JWT_TOKEN" \
  "http://localhost:8000/api/v1/contacts/?search=Іван"

# Пошук за email
curl -H "Authorization: Bearer $JWT_TOKEN" \
  "http://localhost:8000/api/v1/contacts/?search=example.com"

# Пагінація (пропустити 5, показати 10)
curl -H "Authorization: Bearer $JWT_TOKEN" \
  "http://localhost:8000/api/v1/contacts/?skip=5&limit=10"

# Найближчі дні народження (7 днів)
curl -H "Authorization: Bearer $JWT_TOKEN" \
  "http://localhost:8000/api/v1/contacts/birthdays"
```

### Оновлення та видалення

```bash
# Оновити контакт (припустимо ID = 1)
curl -X PUT "http://localhost:8000/api/v1/contacts/1" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "+380509876543",
    "additional_info": "Оновлена інформація про контакт"
  }'

# Отримати конкретний контакт
curl -H "Authorization: Bearer $JWT_TOKEN" \
  "http://localhost:8000/api/v1/contacts/1"

# Видалити контакт
curl -X DELETE "http://localhost:8000/api/v1/contacts/1" \
  -H "Authorization: Bearer $JWT_TOKEN"
```

## 🧪 Комплексне тестування

### Автоматичні тести

```bash
# Запустити всі тести
poetry run pytest

# Тести з детальним виводом
poetry run pytest -v

# Тести з покриттям коду
poetry run pytest --cov=app --cov-report=html

# Тести конкретного модуля
poetry run pytest tests/test_auth.py -v

# Запустити конкретний тест
poetry run pytest tests/test_auth.py::test_user_registration -v

# Тести з live output
poetry run pytest -s
```

### Ручне тестування через Swagger UI

```bash
# Відкрити інтерактивну документацію
open http://localhost:8000/docs

# Крок за кроком:
# 1. Розгорнути POST /api/v1/auth/register
# 2. Натиснути "Try it out"
# 3. Заповнити Request body
# 4. Натиснути "Execute"
# 5. Переглянути Response
```

### Тестування через Postman

```bash
# Імпортувати Postman колекцію (якщо є)
# Або створити нову колекцію з endpoints:

# 1. POST {{base_url}}/api/v1/auth/register
# 2. POST {{base_url}}/api/v1/auth/login
# 3. GET {{base_url}}/api/v1/auth/me
# 4. POST {{base_url}}/api/v1/contacts/
# 5. GET {{base_url}}/api/v1/contacts/

# Де {{base_url}} = http://localhost:8000
```

### Health Check та діагностика

```bash
# Основний health check
curl http://localhost:8000/health

# Перевірити статус сервісів
docker-compose ps

# Переглянути логи
docker-compose logs web
docker-compose logs db
docker-compose logs redis

# Моніторинг ресурсів
docker stats

# Підключитися до контейнера для debugging
docker-compose exec web bash
docker-compose exec db psql -U postgres -d contacts_db
```

## 📊 API Endpoints довідник

### Аутентифікація (`/api/v1/auth`)

| Метод | Endpoint | Опис | Auth | Rate Limit |
|-------|----------|------|------|------------|
| `POST` | `/register` | Реєстрація користувача | ❌ | - |
| `POST` | `/login` | Авторизація | ❌ | - |
| `GET` | `/verify-email` | Верифікація email | ❌ | - |
| `POST` | `/resend-verification` | Повторна відправка верифікації | ❌ | - |
| `GET` | `/me` | Інформація про профіль | ✅ | 10/min |
| `PUT` | `/me` | Оновлення профілю | ✅ | - |
| `POST` | `/me/avatar` | Завантаження аватара | ✅ | - |

### Контакти (`/api/v1/contacts`)

| Метод | Endpoint | Опис | Auth | Pagination |
|-------|----------|------|------|------------|
| `POST` | `/` | Створити контакт | ✅ | - |
| `GET` | `/` | Список контактів | ✅ | ✅ |
| `GET` | `/{id}` | Контакт за ID | ✅ | - |
| `PUT` | `/{id}` | Оновити контакт | ✅ | - |
| `DELETE` | `/{id}` | Видалити контакт | ✅ | - |
| `GET` | `/birthdays` | Дні народження (7 днів) | ✅ | - |

### Системні endpoints

| Метод | Endpoint | Опис |
|-------|----------|------|
| `GET` | `/` | Інформація про API |
| `GET` | `/health` | Health check |
| `GET` | `/docs` | Swagger документація |
| `GET` | `/redoc` | ReDoc документація |

## 🚀 Розгортання у продакшн

### Docker Production Setup

```bash
# 1. Створити production .env
cp .env.example .env.production

# 2. Змінити критичні налаштування
DEBUG=False
SECRET_KEY=super-secure-production-key-64-characters-long
DATABASE_URL=postgresql://user:pass@production-db:5432/contacts_db

# 3. Білд production образу
docker build -f Dockerfile.prod -t contacts-api:prod .

# 4. Запуск з production конфігурацією
docker-compose -f docker-compose.prod.yml up -d
```

### Kubernetes deployment

```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: contacts-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: contacts-api
  template:
    metadata:
      labels:
        app: contacts-api
    spec:
      containers:
      - name: web
        image: contacts-api:prod
        ports:
        - containerPort: 8000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: database-url
```

## 🔧 Troubleshooting

### Поширені проблеми та рішення

#### 1. База даних не підключається
```bash
# Перевірити чи запущений PostgreSQL
docker-compose ps db

# Переглянути логи бази даних
docker-compose logs db

# Перевірити connection string
echo $DATABASE_URL

# Перезапустити сервіс БД
docker-compose restart db
```

#### 2. Email не відправляється
```bash
# Перевірити Gmail налаштування
# Переконатися що 2FA увімкнений
# Перевірити App Password

# Тест email у логах
docker-compose logs web | grep -i mail
```

#### 3. JWT токени не працюють
```bash
# Перевірити SECRET_KEY
echo $SECRET_KEY

# Переконатися що токен не прострочений
# Перевірити формат Authorization header:
# "Authorization: Bearer your-jwt-token"
```

#### 4. Rate limiting блокує запити
```bash
# Перевірити Redis
docker-compose logs redis

# Очистити rate limit cache
docker-compose exec redis redis-cli FLUSHALL
```

### Логи та моніторинг

```bash
# Всі логи
docker-compose logs -f

# Логи конкретного сервісу
docker-compose logs -f web
docker-compose logs -f db
docker-compose logs -f redis

# Останні 100 рядків
docker-compose logs --tail=100 web

# Моніторинг ресурсів
docker stats $(docker-compose ps -q)
```

## 🤝 Розробка та Contributing

### Налаштування dev середовища

```bash
# Форк репозиторію
git clone https://github.com/YOUR-USERNAME/contacts-api.git
cd contacts-api

# Створити нову гілку для фіче
git checkout -b feature/new-feature

# Встановити pre-commit hooks
poetry add --group dev pre-commit
poetry run pre-commit install

# Код style перевірка
poetry run black app tests
poetry run isort app tests
poetry run flake8 app tests
```

### Створення міграцій

```bash
# Після зміни models.py
alembic revision --autogenerate -m "Add new field to Contact model"

# Перевірити створену міграцію
ls alembic/versions/

# Застосувати міграцію
alembic upgrade head

# Відкат міграції
alembic downgrade -1
```

### Додавання нових endpoints

```python
# 1. Додати схему в schemas.py
class NewFeatureSchema(BaseModel):
    name: str
    value: int

# 2. Додати CRUD функцію в crud.py
def create_feature(db: Session, feature: NewFeatureSchema):
    # Implementation

# 3. Додати endpoint в відповідний роутер
@router.post("/features/")
def create_feature_endpoint(
    feature: NewFeatureSchema,
    current_user: User = Depends(get_current_verified_user),
    db: Session = Depends(get_db)
):
    return crud.create_feature(db, feature)

# 4. Додати тести в tests/
def test_create_feature(authenticated_client):
    # Test implementation
```

## 📝 Ліцензія

Цей проект поширюється під ліцензією MIT. Дивіться файл [LICENSE](LICENSE) для деталей.

## 🆘 Підтримка та спільнота

### Отримання допомоги

1. **GitHub Issues** - [створити issue](../../issues/new)
2. **Документація** - перевірте [Wiki](../../wiki)
3. **Email підтримка** - andriiS@fcukspammers.com

### Корисні ресурси

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [SQLAlchemy 2.0 Documentation](https://docs.sqlalchemy.org/en/20/)
- [Pydantic Documentation](https://docs.pydantic.dev/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

**Happy coding! 🚀**
