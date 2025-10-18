# 🌐 Pulse API Gateway

> Центральная точка входа для всех микросервисов системы PulseEMS

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D20.0.0-brightgreen)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.3-blue)](https://www.typescriptlang.org/)

## 📋 О проекте

API Gateway - это единая точка входа для всех микросервисов системы электронного документооборота PulseEMS. Он обеспечивает:

- 🔐 **Аутентификацию и авторизацию** запросов
- 🚦 **Маршрутизацию** запросов к соответствующим микросервисам
- ⚡ **Ограничение частоты запросов** (Rate Limiting)
- 📊 **Логирование** всех запросов
- 🛡️ **Защиту** от распространенных атак
- 🔄 **Load Balancing** между инстансами сервисов

## 🏗️ Архитектура
```
┌─────────────┐
│   Клиент    │
└──────┬──────┘
       │
       ▼
┌─────────────────────────────────┐
│       API Gateway               │
│  ┌──────────────────────────┐  │
│  │  Middleware Pipeline:    │  │
│  │  • CORS                  │  │
│  │  • Rate Limiting         │  │
│  │  • JWT Validation        │  │
│  │  • Request Logging       │  │
│  └──────────────────────────┘  │
└─────────────┬───────────────────┘
              │
    ┌─────────┼─────────┐
    │         │         │
    ▼         ▼         ▼
┌────────┐ ┌──────┐ ┌─────────┐
│  Auth  │ │ User │ │Document │
│Service │ │Service│ │ Service │
└────────┘ └──────┘ └─────────┘
```

## 🚀 Маршруты

| Путь | Целевой сервис | Требуется Auth |
|------|---------------|----------------|
| `/api/auth/*` | Auth Service | ❌ |
| `/api/users/*` | User Service | ✅ |
| `/api/documents/*` | Document Service | ✅ |
| `/api/files/*` | File Storage Service | ✅ |
| `/api/workflow/*` | Workflow Service | ✅ |
| `/api/notifications/*` | Notification Service | ✅ |
| `/api/patients/*` | Patient Service | ✅ |
| `/health` | Health Check | ❌ |

## 🛠️ Технологический стек

- **Runtime:** Node.js 20+
- **Framework:** Express.js
- **Language:** TypeScript
- **HTTP Proxy:** http-proxy-middleware
- **Authentication:** JWT (jsonwebtoken)
- **Rate Limiting:** express-rate-limit
- **Security:** Helmet
- **Logging:** Winston
- **Validation:** Joi

## 📦 Установка

### Предварительные требования

- Node.js >= 20.0.0
- npm >= 9.0.0
- Docker (опционально)

### Локальная разработка

1. **Клонировать репозиторий:**
```bash
git clone https://github.com/PulseEMS/pulse-api-gateway.git
cd pulse-api-gateway
```

2. **Установить зависимости:**
```bash
npm install
```

3. **Настроить переменные окружения:**
```bash
cp .env.example .env
# Отредактируйте .env файл
```

4. **Запустить в режиме разработки:**
```bash
npm run dev
```

5. **Проверить работу:**
```bash
curl http://localhost:3000/health
```

### Запуск с Docker
```bash
# Сборка образа
docker build -t pulseems/api-gateway .

# Запуск контейнера
docker run -p 3000:3000 --env-file .env pulseems/api-gateway
```

### Запуск с Docker Compose
```bash
docker-compose up -d
```

## 📝 Переменные окружения

Создайте файл `.env` на основе `.env.example`:
```env
# Приложение
NODE_ENV=development
PORT=3000
APP_NAME=PulseEMS API Gateway

# JWT
JWT_SECRET=ваш-секретный-ключ-измените-в-продакшене
JWT_EXPIRES_IN=1d

# Ограничение запросов
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# CORS
CORS_ORIGIN=http://localhost:3001

# URL сервисов
AUTH_SERVICE_URL=http://localhost:3001
USER_SERVICE_URL=http://localhost:3002
DOCUMENT_SERVICE_URL=http://localhost:3003
FILE_STORAGE_SERVICE_URL=http://localhost:3004
WORKFLOW_SERVICE_URL=http://localhost:3005
NOTIFICATION_SERVICE_URL=http://localhost:3006
PATIENT_SERVICE_URL=http://localhost:3007

# Логирование
LOG_LEVEL=info
```

## 🔧 Скрипты
```bash
# Разработка с hot-reload
npm run dev

# Сборка проекта
npm run build

# Запуск production версии
npm start

# Запуск production с NODE_ENV
npm run start:prod

# Тестирование
npm test

# Тесты с покрытием
npm run test:coverage

# Тесты в watch режиме
npm run test:watch

# Линтинг
npm run lint

# Автоисправление линтинга
npm run lint:fix

# Форматирование кода
npm run format

# Docker сборка
npm run docker:build

# Docker запуск
npm run docker:run
```

## 📂 Структура проекта
```
pulse-api-gateway/
│
├── src/
│   ├── config/                  # Конфигурация
│   │   ├── index.ts
│   │   ├── services.config.ts
│   │   └── routes.config.ts
│   │
│   ├── middleware/              # Middleware
│   │   ├── auth.middleware.ts
│   │   ├── rate-limit.middleware.ts
│   │   ├── logger.middleware.ts
│   │   ├── cors.middleware.ts
│   │   └── error-handler.middleware.ts
│   │
│   ├── routes/                  # Маршруты
│   │   ├── index.ts
│   │   ├── auth.routes.ts
│   │   ├── users.routes.ts
│   │   ├── documents.routes.ts
│   │   ├── patients.routes.ts
│   │   └── health.routes.ts
│   │
│   ├── services/                # Сервисы
│   │   └── proxy.service.ts
│   │
│   ├── utils/                   # Утилиты
│   │   ├── logger.ts
│   │   ├── response.ts
│   │   └── validators.ts
│   │
│   ├── types/                   # TypeScript типы
│   │   └── index.ts
│   │
│   ├── app.ts                   # Express приложение
│   └── server.ts                # Точка входа
│
├── tests/                       # Тесты
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── docs/                        # Документация
│   ├── API.md
│   ├── ARCHITECTURE.md
│   └── DEPLOYMENT.md
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
│
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── .env.example
├── .gitignore
├── .eslintrc.js
├── .prettierrc
├── tsconfig.json
├── package.json
├── nodemon.json
└── README.md
```

## 🔐 Безопасность

API Gateway реализует следующие механизмы безопасности:

### Аутентификация
- JWT токены для проверки пользователей
- Автоматическая проверка токенов для защищенных маршрутов
- Поддержка refresh tokens

### Защита от атак
- **Helmet** - защита HTTP заголовков
- **Rate Limiting** - ограничение количества запросов
- **CORS** - контроль cross-origin запросов
- **Input Validation** - валидация входящих данных

### Логирование
- Логирование всех запросов
- Логирование ошибок
- Audit trail для критичных операций

## 🧪 Тестирование
```bash
# Запуск всех тестов
npm test

# Тесты с покрытием
npm run test:coverage

# Unit тесты
npm run test:unit

# Integration тесты
npm run test:integration

# E2E тесты
npm run test:e2e
```

### Пример теста
```typescript
describe('API Gateway', () => {
  it('должен проксировать запрос к auth service', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'test@example.com',
        password: 'password123'
      });
    
    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('token');
  });
});
```

## 📊 Мониторинг

### Health Check
```bash
curl http://localhost:3000/health
```

Ответ:
```json
{
  "status": "healthy",
  "service": "api-gateway",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "uptime": 3600
}
```

### Метрики

- Время ответа сервисов
- Количество запросов
- Количество ошибок
- Использование памяти

## 🚀 Развертывание

### Production с Docker
```bash
# Сборка production образа
docker build -t pulseems/api-gateway:latest .

# Запуск
docker run -d \
  --name pulseems-api-gateway \
  -p 3000:3000 \
  --env-file .env.production \
  pulseems/api-gateway:latest
```

### Kubernetes
```bash
kubectl apply -f k8s/deployment.yml
kubectl apply -f k8s/service.yml
kubectl apply -f k8s/ingress.yml
```

## 📚 Документация

- [Архитектура](docs/ARCHITECTURE.md) - подробное описание архитектуры
- [API](docs/API.md) - документация API
- [Развертывание](docs/DEPLOYMENT.md) - инструкции по развертыванию

## 🔗 Связанные репозитории

- [pulse-infrastructure](https://github.com/PulseEMS/pulse-infrastructure) - Инфраструктура
- [pulse-auth-service](https://github.com/PulseEMS/pulse-auth-service) - Сервис аутентификации
- [pulse-document-service](https://github.com/PulseEMS/pulse-document-service) - Сервис документов

## 🤝 Contributing

Читайте [CONTRIBUTING.md](https://github.com/PulseEMS/.github/blob/main/CONTRIBUTING.md) для информации о процессе разработки.

## 📝 Changelog

Все значимые изменения документируются в [CHANGELOG.md](CHANGELOG.md).

## 📄 Лицензия

MIT License - см. [LICENSE](LICENSE) файл для деталей.

## 👨‍💻 Автор

**Дипломная работа**  
Тема: "Проектирование и разработка системы электронного документооборота медицинского учреждения с асинхронным обменом данными"

---

<div align="center">

**[PulseEMS Organization](https://github.com/PulseEMS)** | **[Документация](https://github.com/PulseEMS/pulse-infrastructure)** | **[Issues](https://github.com/PulseEMS/pulse-api-gateway/issues)**

⭐ Если проект полезен, поставьте звездочку!

</div>
```

---

## Topics (теги) для репозитория на русском
```
pulse-ems
api-gateway
микросервисы
nodejs
typescript
express
маршрутизация
медицина
электронный-документооборот
jwt
rate-limiting
