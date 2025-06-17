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

### Backend
- **FastAPI 0.104+** - сучасний веб-фреймворк для Python
- **SQLAlchemy 2.0+** - ORM для роботи з базою даних
- **PostgreSQL** - реляційна база даних
- **Pydantic 2.0+** - валідація та серіалізація даних
- **Alembic** - міграції бази даних

### Аутентифікація та безпека
- **python-jose** - JWT токени з шифруванням
- **passlib + bcrypt** - безпечне хешування паролів
- **slowapi + Redis** - rate limiting
- **python-multipart** - обробка форм та файлів

### Зовнішні сервіси
- **Cloudinary** - зберігання та обробка зображень
- **FastAPI-Mail** - відправка email листів
- **Redis** - кешування та rate limiting

### Розробка та тестування
- **Poetry** - управління залежностями
- **pytest + httpx** - тестування API
- **Faker** - генерація тестових даних
- **Docker + Docker Compose** - контейнеризація

## 📁 Структура проекту

```
contacts-api/
├── app/                          # Основний код додатку
│   ├── main.py                   # FastAPI додаток з middleware
│   ├── database.py               # Налаштування SQLAlchemy
│   ├── models.py                 # Моделі: User, Contact, EmailVerification
│   ├── schemas.py                # Pydantic схеми для валідації
│   ├── crud.py                   # CRUD операції з аутентифікацією
│   ├── auth.py                   # JWT токени та авторизація
│   ├── email.py                  # Сервіс відправки email
│   ├── cloudinary_service.py     # Завантаження аватарів
│   ├── rate_limiter.py           # Rate limiting налаштування
│   └── routers/                  # API роутери
│       ├── auth.py               # Реєстрація, логін, профіль
│       └── contacts.py           # CRUD операції контактів
├── tests/                        # Повне тестове покриття
│   ├── conftest.py               # Фікстури та налаштування
│   ├── test_auth.py              # Тести аутентифікації
│   └── test_contacts.py          # Тести API контактів
├── scripts/                      # Допоміжні скрипти
│   └── seed_data.py              # Генерація тестових даних
├── alembic/                      # Міграції бази даних
├── .env.example                  # Шаблон змінних середовища
├── docker-compose.yml            # Оркестрація контейнерів
├── pyproject.toml                # Poetry конфігурація
└── README.md                     # Документація
```

## ⚡ Швидкий старт

### Метод 1: Docker Compose (рекомендовано)

```bash
# 1. Клонувати репозиторій
git clone <repository-url>
cd contacts-api

# 2. Налаштувати змінні середовища
cp .env.example .env
# Відредагуйте .env файл з вашими налаштуваннями

# 3. Запустити всі сервіси
docker-compose up -d

# 4. Створити тестові дані (опціонально)
docker-compose exec web python scripts/seed_data.py

# 5. Відкрити документацію API
open http://localhost:8000/docs
```

### Метод 2: Локальний розвиток

```bash
# 1. Встановити Poetry
curl -sSL https://install.python-poetry.org | python3 -

# 2. Встановити залежності
poetry install

# 3. Налаштувати змінні середовища
cp .env.example .env

# 4. Запустити PostgreSQL та Redis
docker-compose up -d db redis

# 5. Активувати віртуальне середовище
poetry shell

# 6. Застосувати міграції (якщо потрібно)
alembic upgrade head

# 7. Створити тестові дані
python scripts/seed_data.py

# 8. Запустити додаток
uvicorn app.main:app --reload
```

## 🔐 Аутентифікація та авторизація

### Реєстрація нового користувача

```bash
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

**Відповідь:**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "is_verified": false,
  "created_at": "2024-01-15T10:30:00"
}
```

### Верифікація email

Після реєстрації користувач отримає лист з посиланням для верифікації:
```
http://localhost:8000/api/v1/auth/verify-email?token=<verification_token>
```

### Авторизація (логін)

```bash
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
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### Використання токена

Всі наступні запити повинні включати JWT токен:

```bash
# Отримання інформації про профіль
curl -X GET "http://localhost:8000/api/v1/auth/me" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"

# Створення контакту
curl -X POST "http://localhost:8000/api/v1/contacts/" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Іван",
    "last_name": "Петренко",
    "email": "ivan@example.com",
    "phone": "+380501234567",
    "birthday": "1990-05-15"
  }'
```

## 📋 API Endpoints

### Аутентифікація (`/api/v1/auth`)

| Метод | Endpoint | Опис | Аутентифікація |
|-------|----------|------|----------------|
| `POST` | `/register` | Реєстрація користувача | ❌ |
| `POST` | `/login` | Авторизація | ❌ |
| `GET` | `/verify-email` | Верифікація email | ❌ |
| `POST` | `/resend-verification` | Повторна відправка верифікації | ❌ |
| `GET` | `/me` | Інформація про профіль | ✅ |
| `PUT` | `/me` | Оновлення профілю | ✅ |
| `POST` | `/me/avatar` | Завантаження аватара | ✅ |

### Контакти (`/api/v1/contacts`)

| Метод | Endpoint | Опис | Аутентифікація |
|-------|----------|------|----------------|
| `POST` | `/` | Створити контакт | ✅ |
| `GET` | `/` | Список контактів | ✅ |
| `GET` | `/{id}` | Контакт за ID | ✅ |
| `PUT` | `/{id}` | Оновити контакт | ✅ |
| `DELETE` | `/{id}` | Видалити контакт | ✅ |
| `GET` | `/birthdays` | Дні народження (7 днів) | ✅ |

### Query параметри для пошуку

- `search` - пошук за ім'ям, прізвищем або email
- `skip` - кількість записів для пропуску (пагінація)
- `limit` - максимальна кількість записів (макс. 1000)

## 💡 Приклади використання

### Повний workflow користувача

```bash
# 1. Реєстрація
REGISTER_RESPONSE=$(curl -s -X POST "http://localhost:8000/api/v1/auth/register" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123",
    "first_name": "Test",
    "last_name": "User"
  }')

# 2. Симулювати верифікацію email (в реальності - через лист)
# curl "http://localhost:8000/api/v1/auth/verify-email?token=<від_email>"

# 3. Логін
TOKEN_RESPONSE=$(curl -s -X POST "http://localhost:8000/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }')

TOKEN=$(echo $TOKEN_RESPONSE | jq -r '.access_token')

# 4. Створення контакту
curl -X POST "http://localhost:8000/api/v1/contacts/" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Іван",
    "last_name": "Петренко",
    "email": "ivan@example.com",
    "phone": "+380501234567",
    "birthday": "1990-05-15",
    "additional_info": "Важливий клієнт"
  }'

# 5. Пошук контактів
curl "http://localhost:8000/api/v1/contacts/?search=Іван" \
  -H "Authorization: Bearer $TOKEN"

# 6. Дні народження
curl "http://localhost:8000/api/v1/contacts/birthdays" \
  -H "Authorization: Bearer $TOKEN"
```

### Завантаження аватара

```bash
curl -X POST "http://localhost:8000/api/v1/auth/me/avatar" \
  -H "Authorization: Bearer $TOKEN" \
  -F "file=@/path/to/avatar.jpg"
```

## 🔧 Налаштування

### Змінні середовища (.env)

```env
# База даних
DATABASE_URL=postgresql://postgres:password@localhost:5432/contacts_db

# JWT налаштування
SECRET_KEY=your-super-secret-jwt-key-change-in-production
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Email налаштування
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password
MAIL_FROM=noreply@contacts-api.com
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587

# Cloudinary налаштування
CLOUDINARY_CLOUD_NAME=your-cloudinary-cloud-name
CLOUDINARY_API_KEY=your-cloudinary-api-key
CLOUDINARY_API_SECRET=your-cloudinary-api-secret

# Redis налаштування
REDIS_HOST=localhost
REDIS_PORT=6379

# CORS налаштування
ALLOWED_ORIGINS=http://localhost:3000,https://yourdomain.com

# Додаток
DEBUG=True
HOST=0.0.0.0
PORT=8000
```

### Налаштування Gmail для email

1. Увімкніть двофакторну аутентифікацію в Gmail
2. Створіть пароль додатку: https://myaccount.google.com/apppasswords
3. Використовуйте цей пароль в `MAIL_PASSWORD`

### Налаштування Cloudinary

1. Зареєструйтеся на https://cloudinary.com
2. Отримайте API ключі з Dashboard
3. Додайте їх до `.env` файлу

## 🧪 Тестування

### Запуск тестів

```bash
# Всі тести
poetry run pytest

# З покриттям коду
poetry run pytest --cov=app --cov-report=html

# Тести аутентифікації
poetry run pytest tests/test_auth.py -v

# Тести контактів
poetry run pytest tests/test_contacts.py -v
```

### Ручне тестування

```bash
# Health check
curl http://localhost:8000/health

# Документація API
open http://localhost:8000/docs

# ReDoc документація
open http://localhost:8000/redoc
```

## 📊 Моніторинг та логування

### Rate Limiting

API має вбудований rate limiting:
- Загальні endpoints: стандартні ліміти
- Аутентифікація: 10 запитів на хвилину для `/auth/me`

### Логування

Додаток логує:
- HTTP запити та відповіді
- Помилки аутентифікації
- Помилки бази даних
- Валідаційні помилки

## 🐳 Docker інформація

### Сервіси

- **web** - FastAPI додаток (порт 8000)
- **db** - PostgreSQL база даних (порт 5432)
- **redis** - Redis кеш (порт 6379)
- **adminer** - веб-інтерфейс для БД (порт 8080)

### Корисні Docker команди

```bash
# Перегляд логів
docker-compose logs -f web

# Перезапуск сервісу
docker-compose restart web

# Виконання команд в контейнері
docker-compose exec web python scripts/seed_data.py

# Очищення даних
docker-compose down -v
```

## 🚀 Розгортання

### Продакшн рекомендації

1. **Безпека:**
   - Змініть `SECRET_KEY` на криптографічно безпечний
   - Використовуйте HTTPS
   - Налаштуйте CORS для конкретних доменів
   - Використовуйте сильні паролі для БД

2. **База даних:**
   - Використовуйте керований PostgreSQL сервіс
   - Налаштуйте регулярні backup'и
   - Моніторинг продуктивності

3. **Масштабування:**
   - Використовуйте reverse proxy (nginx)
   - Налаштуйте load balancing
   - Моніторинг ресурсів та логів

4. **Email та файли:**
   - Налаштуйте надійний SMTP сервіс
   - Використовуйте CDN для зображень
   - Backup'и файлів

## 🤝 Розробка

### Встановлення для розробки

```bash
# Клонування та налаштування
git clone <repo> && cd contacts-api
poetry install
cp .env.example .env

# Запуск у режимі розробки
docker-compose up -d db redis
poetry shell
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Створення міграцій

```bash
# Створення нової міграції
alembic revision --autogenerate -m "Add new field"

# Застосування міграцій
alembic upgrade head

# Відкат міграції
alembic downgrade -1
```

### Додавання нових залежностей

```bash
# Додавання основної залежності
poetry add package-name

# Додавання dev залежності
poetry add --group dev package-name
```

## 📝 Ліцензія

Цей проект поширюється під ліцензією MIT.

## 🆘 Підтримка

Якщо виникли питання або проблеми:

1. Перевірте [Issues](../../issues) на GitHub
2. Створіть новий Issue з детальним описом
3. Надайте логи та налаштування

**Контакти розробника:** andriiS@fcukspammers.com
