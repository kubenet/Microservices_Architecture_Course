# Практическая работа №5
## Тема: Документирование систем с микросервисной архитектурой

**Дисциплина:** Микросервисная архитектура
**Объём:** 2 академических часа (+ самостоятельная работа)
**Формируемая компетенция:** ПК-4
**Целевой проект:** [microservices-flask](https://github.com/antkahn/flask-api-starter-kit) *(или аналогичный шаблон, подготовленный преподавателем)*

---

## 1. Цель работы

Получить практические навыки анализа, создания и оформления документации для проекта с микросервисной архитектурой на базе **Python / FastAPI**. На примере простого проекта из **4 микросервисов** с брокером **RabbitMQ** и кэшем **Redis** научиться:

- анализировать существующую документацию и выявлять её пробелы;
- составлять README сервиса по стандартному шаблону;
- описывать REST API-контракты в формате **OpenAPI 3.x**;
- описывать асинхронные события в формате **AsyncAPI**;
- фиксировать архитектурные решения в формате **ADR**;
- формировать **Runbook** для эксплуатации;
- визуализировать архитектуру с помощью модели **C4** (диаграммы в формате Mermaid).

---

## 2. Характеристика целевого проекта

**Проект** — упрощённый интернет-магазин с асинхронным взаимодействием сервисов через брокер сообщений.

| Параметр | Значение |
|---|---|
| Количество сервисов | **4** |
| Язык программирования | **Python 3.11+** |
| Веб-фреймворк | **FastAPI** |
| Брокер сообщений | **RabbitMQ** |
| Кэш / хранилище сессий | **Redis** |
| СУБД | **PostgreSQL** (через SQLAlchemy) |
| Оркестрация | **Docker Compose** |
| Протокол взаимодействия | **REST API** (HTTP/JSON) + **AMQP** (асинхронные события) |

### Архитектура проекта

```
┌──────────────────────────────────────────────────────────────────────┐
│                        Docker Compose                                │
│                                                                      │
│  ┌─────────────┐   HTTP    ┌─────────────────┐                      │
│  │  Клиент     │──────────▶│  order-service   │                      │
│  │  (браузер / │◀──────────│  (FastAPI)       │                      │
│  │   curl)     │           │  порт: 8000      │                      │
│  └─────────────┘           └────────┬────────┘                       │
│                                     │                                │
│                              publish │ event:                       │
│                              order.created                           │
│                                     ▼                                │
│                           ┌─────────────────┐                        │
│                           │    RabbitMQ      │                        │
│                           │    порт: 5672    │                        │
│                           └────────┬────────┘                        │
│                                    │                                 │
│                     consume        │        consume                  │
│                 ┌──────────────────┼──────────────────┐              │
│                 ▼                  │                  ▼              │
│  ┌──────────────────────┐         │   ┌──────────────────────┐      │
│  │  notification-service │         │   │  analytics-service   │      │
│  │  (FastAPI)            │         │   │  (FastAPI)           │      │
│  │  порт: 8001           │         │   │  порт: 8002          │      │
│  └──────────────────────┘         │   └──────────────────────┘      │
│                                   │                                  │
│  ┌──────────────────────┐         │                                  │
│  │  product-service      │◀────────┘                                  │
│  │  (FastAPI)            │   HTTP (запрос каталога)                   │
│  │  порт: 8003           │                                            │
│  │  ┌──────────┐        │                                            │
│  │  │  Redis   │        │                                            │
│  │  │  порт:   │        │                                            │
│  │  │  6379    │        │                                            │
│  │  └──────────┘        │                                            │
│  └──────────────────────┘                                            │
│                                                                      │
│  ┌──────────────────────┐                                            │
│  │  PostgreSQL           │                                            │
│  │  порт: 5432           │                                            │
│  └──────────────────────┘                                            │
└──────────────────────────────────────────────────────────────────────┘
```

### Перечень сервисов проекта

| № | Сервис | Назначение | Порт | Внешние зависимости |
|---|---|---|---|---|
| 1 | `order-service` | Создание и управление заказами | 8000 | PostgreSQL, RabbitMQ, product-service |
| 2 | `product-service` | Каталог товаров, кэширование в Redis | 8003 | PostgreSQL, Redis |
| 3 | `notification-service` | Отправка уведомлений (подписчик событий) | 8001 | RabbitMQ |
| 4 | `analytics-service` | Сбор аналитики по заказам (подписчик событий) | 8002 | RabbitMQ, PostgreSQL |

### Инфраструктурные компоненты

| Компонент | Назначение | Порт |
|---|---|---|
| **RabbitMQ** | Асинхронный обмен событиями | 5672 (AMQP), 15672 (UI) |
| **Redis** | Кэширование каталога товаров | 6379 |
| **PostgreSQL** | Основное хранилище данных | 5432 |

---

## 3. Предварительные требования

Перед началом работы необходимо:

- Установленные **Git**, **Docker**, **docker-compose**;
- Аккаунт на [github.com](https://github.com);
- Текстовый редактор с поддержкой Markdown (рекомендуется **VS Code**);
- Знакомство с материалами **Лекции 6** «Документирование систем с МСА»;
- Базовое понимание **FastAPI**, **REST API**, **Docker Compose**.

### Клонирование и запуск проекта

```bash
# Клонирование репозитория
git clone https://github.com/<преподаватель>/fastapi-microservices-demo.git
cd fastapi-microservices-demo

# Запуск всех сервисов
docker-compose up -d

# Проверка работоспособности
curl http://localhost:8000/health    # order-service
curl http://localhost:8001/health    # notification-service
curl http://localhost:8002/health    # analytics-service
curl http://localhost:8003/health    # product-service
```

### Структура репозитория

```
fastapi-microservices-demo/
├── docker-compose.yml
├── .env.example
├── README.md
│
├── services/
│   ├── order_service/
│   │   ├── app/
│   │   │   ├── main.py            # FastAPI приложение
│   │   │   ├── models.py          # SQLAlchemy модели
│   │   │   ├── schemas.py         # Pydantic схемы
│   │   │   ├── routes/
│   │   │   │   ├── orders.py      # CRUD заказы
│   │   │   │   └── health.py      # Health check
│   │   │   ├── services/
│   │   │   │   ├── order_service.py
│   │   │   │   └── publisher.py   # Публикация в RabbitMQ
│   │   │   └── core/
│   │   │       ├── config.py      # Настройки из .env
│   │   │       └── database.py    # Подключение к БД
│   │   ├── Dockerfile
│   │   ├── requirements.txt
│   │   └── openapi.yaml           # Спецификация API
│   │
│   ├── product_service/
│   │   ├── app/
│   │   │   ├── main.py
│   │   │   ├── models.py
│   │   │   ├── routes/
│   │   │   ├── services/
│   │   │   │   └── cache.py       # Логика кэширования в Redis
│   │   │   └── core/
│   │   ├── Dockerfile
│   │   ├── requirements.txt
│   │   └── openapi.yaml
│   │
│   ├── notification_service/
│   │   ├── app/
│   │   │   ├── main.py
│   │   │   ├── consumer.py        # Подписка на события из RabbitMQ
│   │   │   └── core/
│   │   ├── Dockerfile
│   │   └── requirements.txt
│   │
│   └── analytics_service/
│       ├── app/
│       │   ├── main.py
│       │   ├── consumer.py
│       │   └── core/
│       ├── Dockerfile
│       └── requirements.txt
│
├── infra/
│   ├── rabbitmq/
│   │   └── definitions.json       # Очереди, exchange, bindings
│   ├── redis/
│   └── postgres/
│       └── init.sql
│
└── docs/
    ├── architecture.md
    └── runbooks/
```

---

## 4. Задания практической работы

---

### Задание 1. Аудит существующей документации
**Форма:** аудиторно | **Время:** 15 мин

**Цель:** оценить полноту и качество документации проекта по модели Diátaxis.

**Порядок выполнения:**

1. Откройте `README.md` в корне репозитория и папку `docs/`.
2. Пройдите по документации каждого сервиса в `services/<имя_сервиса>/`.
3. Заполните таблицу аудита:

| Критерий | Есть / Нет | Качество (1–5) | Комментарий |
|---|---|---|---|
| Общее описание проекта | | | |
| Диаграмма архитектуры | | | |
| Инструкции по локальному запуску (`docker-compose up`) | | | |
| README для каждого сервиса | | | |
| Спецификация OpenAPI для REST-эндпоинтов | | | |
| Описание событий в RabbitMQ (очереди, форматы сообщений) | | | |
| Описание конфигурации (переменные окружения, `.env`) | | | |
| ADR или описание архитектурных решений | | | |
| Runbook / troubleshooting | | | |
| Changelog | | | |

**Контрольный вопрос:** Какие разделы отсутствуют или недостаточно раскрыты? Составьте список из **не менее 4** выявленных пробелов.

---

### Задание 2. Составление README для выбранного сервиса
**Форма:** аудиторно | **Время:** 25 мин

**Цель:** научиться создавать стандартизированную документацию сервиса.

**Порядок выполнения:**

1. Выберите **один** сервис по указанию преподавателя:

| Вариант | Сервис | Фокус задания |
|---|---|---|
| А | `order-service` | Оркестрация, публикация событий, REST API |
| Б | `product-service` | Кэширование в Redis, CRUD-операции |
| В | `notification-service` | Подписка на события, асинхронная обработка |
| Г | `analytics-service` | Подписка на события, агрегация данных |

2. Изучите исходный код выбранного сервиса:
   - `app/main.py` — точки входа, middleware;
   - `app/routes/` — REST-эндпоинты;
   - `app/services/` — бизнес-логика;
   - `app/core/config.py` — переменные окружения;
   - `Dockerfile` и `requirements.txt`.

3. Создайте файл `README_STUDENT.md` в корне выбранного сервиса по шаблону:

```markdown
# <название сервиса>

## Описание
(1–2 абзаца: назначение, бизнес-роль в системе)

## Архитектурная роль
(Место в общей архитектуре. Ссылка на диаграмму)

## Быстрый старт

### Требования
- Python 3.11+
- Docker и docker-compose

### Запуск локально (без Docker)
```bash
cd services/<имя_сервиса>
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port <порт>
```

### Запуск через Docker
```bash
docker-compose up -d <имя_сервиса>
```

### Проверка
```bash
curl http://localhost:<порт>/health
```

## REST API
| Метод | Путь | Описание | Тело запроса | Ответ |
|---|---|---|---|---|
| `GET` | `/health` | Проверка живости | — | `{"status": "ok"}` |
| `POST` | `/orders` | Создание заказа | `CreateOrderRequest` | `Order` |
| ... | ... | ... | ... | ... |

## Асинхронные события (если применимо)
### Публикуемые события
| Событие | Очередь / Exchange | Формат сообщения |
|---|---|---|
| `order.created` | `orders` / `orders.exchange` | JSON (см. ниже) |

### Потребляемые события
| Событие | Очередь | Обработчик |
|---|---|---|
| `order.created` | `notification.order.created` | `consumer.py::handle_order_created` |

## Зависимости
| Зависимость | Тип | Назначение |
|---|---|---|
| PostgreSQL | Хранилище | Данные заказов |
| RabbitMQ | Брокер сообщений | Публикация событий |
| product-service | HTTP (REST) | Запрос информации о товарах |

## Конфигурация
| Переменная | Назначение | По умолчанию | Обязательная |
|---|---|---|---|
| `DATABASE_URL` | Строка подключения к PostgreSQL | — | Да |
| `RABBITMQ_URL` | Строка подключения к RabbitMQ | `amqp://guest:guest@localhost:5672/` | Да |
| `REDIS_URL` | Строка подключения к Redis | `redis://localhost:6379/0` | Нет |
| `SERVICE_PORT` | Порт сервиса | `8000` | Нет |

## Тестирование
```bash
cd services/<имя_сервиса>
pytest tests/ -v
```

## Контакты
Команда-владелец: ...
```

**Контрольный вопрос:** Какие разделы вызвали наибольшие затруднения и почему?

---

### Задание 3. Описание REST API и асинхронных событий
**Форма:** аудиторно | **Время:** 20 мин

**Цель:** зафиксировать контракты синхронного и асинхронного взаимодействия.

#### Часть 3а. Спецификация OpenAPI (REST API)

1. Изучите роуты выбранного сервиса в `app/routes/`.
2. Составьте спецификацию **OpenAPI 3.0** в формате YAML:

```yaml
openapi: 3.0.3
info:
  title: Order Service API
  version: 1.0.0
  description: >
    Сервис управления заказами.
    Команда-владелец: orders-team.

servers:
  - url: http://localhost:8000
    description: Локальная разработка

paths:
  /health:
    get:
      summary: Проверка живости сервиса
      operationId: healthCheck
      responses:
        '200':
          description: Сервис работает
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                    example: ok

  /orders:
    post:
      summary: Создание нового заказа
      operationId: createOrder
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
      responses:
        '201':
          description: Заказ создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '422':
          description: Ошибка валидации

    get:
      summary: Список заказов
      operationId: listOrders
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: size
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Список заказов

  /orders/{order_id}:
    get:
      summary: Получить заказ по ID
      operationId: getOrder
      parameters:
        - name: order_id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Данные заказа
        '404':
          description: Заказ не найден

components:
  schemas:
    CreateOrderRequest:
      type: object
      required:
        - items
        - customer_email
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
        customer_email:
          type: string
          format: email

    OrderItem:
      type: object
      properties:
        product_id:
          type: integer
        quantity:
          type: integer
          minimum: 1

    Order:
      type: object
      properties:
        id:
          type: integer
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
        total_amount:
          type: number
        status:
          type: string
          enum: [created, confirmed, shipped, delivered, cancelled]
        created_at:
          type: string
          format: date-time
```

3. Проверьте спецификацию через [Swagger Editor](https://editor.swagger.io/) или [Redocly](https://redocly.com/redoc).

#### Часть 3б. Спецификация AsyncAPI (события RabbitMQ)

1. Изучите файлы `publisher.py` и `consumer.py`.
2. Составьте спецификацию **AsyncAPI 2.6**:

```yaml
asyncapi: 2.6.0
info:
  title: Order Events
  version: 1.0.0
  description: >
    События, публикуемые и потребляемые сервисами
    через RabbitMQ.

servers:
  rabbitmq:
    url: amqp://localhost:5672
    protocol: amqp

channels:
  order.created:
    publish:
      operationId: publishOrderCreated
      message:
        $ref: '#/components/messages/OrderCreated'
    subscribe:
      operationId: consumeOrderCreated
      message:
        $ref: '#/components/messages/OrderCreated'

  order.cancelled:
    publish:
      operationId: publishOrderCancelled
      message:
        $ref: '#/components/messages/OrderCancelled'

components:
  messages:
    OrderCreated:
      name: OrderCreated
      title: Событие создания заказа
      contentType: application/json
      payload:
        $ref: '#/components/schemas/OrderCreatedPayload'

    OrderCancelled:
      name: OrderCancelled
      title: Событие отмены заказа
      contentType: application/json
      payload:
        type: object
        properties:
          order_id:
            type: integer
          reason:
            type: string
          cancelled_at:
            type: string
            format: date-time

  schemas:
    OrderCreatedPayload:
      type: object
      required:
        - order_id
        - customer_email
        - total_amount
        - created_at
      properties:
        order_id:
          type: integer
        customer_email:
          type: string
          format: email
        items:
          type: array
          items:
            type: object
            properties:
              product_id:
                type: integer
              product_name:
                type: string
              quantity:
                type: integer
              price:
                type: number
        total_amount:
          type: number
        created_at:
          type: string
          format: date-time
```

**Контрольный вопрос:** Чем отличается подход к документированию синхронного REST-вызова от асинхронного события в RabbitMQ? Какие дополнительные сведения необходимы для событий?

---

### Задание 4. Диаграмма архитектуры в модели C4
**Форма:** самостоятельно | **Время:** 20 мин

**Цель:** визуализировать архитектуру проекта на уровнях Context и Container.

#### Часть 4а. Уровень 1 — Context (система как целое)

```mermaid
graph TB
    Customer((Покупатель))
    Admin((Администратор))

    subgraph Система интернет-магазина
        direction TB
        MS[Микросервисная система]
    end

    Customer -->|Оформляет заказы| MS
    Admin -->|Управляет каталогом| MS
```

#### Часть 4б. Уровень 2 — Container (сервисы и инфраструктура)

Составьте полную диаграмму, включив **все 4 сервиса**, **RabbitMQ**, **Redis**, **PostgreSQL** и протоколы взаимодействия:

```mermaid
graph TB
    Client((Клиент)) -->|HTTP/REST| OS[order-service<br/>Python / FastAPI<br/>порт 8000]

    OS -->|HTTP/REST<br/>запрос товаров| PS[product-service<br/>Python / FastAPI<br/>порт 8003]
    OS -->|publish: order.created<br/>order.cancelled| MQ[RabbitMQ<br/>порт 5672]
    OS --> DB[(PostgreSQL<br/>порт 5432)]

    PS --> DB
    PS --> REDIS[(Redis<br/>порт 6379)]

    MQ -->|consume: order.created| NS[notification-service<br/>Python / FastAPI<br/>порт 8001]
    MQ -->|consume: order.created<br/>order.cancelled| AS[analytics-service<br/>Python / FastAPI<br/>порт 8002]

    AS --> DB

    style OS fill:#4CAF50,color:#fff,stroke:#333
    style PS fill:#2196F3,color:#fff,stroke:#333
    style NS fill:#FF9800,color:#fff,stroke:#333
    style AS fill:#9C27B0,color:#fff,stroke:#333
    style MQ fill:#FF5722,color:#fff,stroke:#333
    style REDIS fill:#f44336,color:#fff,stroke:#333
    style DB fill:#3F51B5,color:#fff,stroke:#333
```

4. Сохраните диаграммы в файлы `.mmd` и добавьте в отчёт.

**Контрольный вопрос:** Что произойдёт, если `RabbitMQ` будет недоступен в момент создания заказа? Какие сервисы пострадают и как это отразится на пользователе?

---

### Задание 5. Запись ADR
**Форма:** самостоятельно | **Время:** 20 мин

**Цель:** зафиксировать и обосновать архитектурное решение проекта.

**Порядок выполнения:**

1. Выберите **одну** тему из списка ниже.
2. Изучите соответствующий код и конфигурацию.
3. Оформите ADR по шаблону.

**Темы для ADR (на выбор):**

| № | Тема | Что анализировать |
|---|---|---|
| 1 | Почему выбран RabbitMQ вместо Redis Pub/Sub для асинхронных событий | `docker-compose.yml`, `publisher.py`, `consumer.py` |
| 2 | Почему `product-service` кэширует данные в Redis, а не читает каждый раз из PostgreSQL | `cache.py`, `config.py` |
| 3 | Почему выбран синхронный HTTP-вызов к `product-service` вместо асинхронного события | `order_service.py`, маршруты |
| 4 | Почему все сервисы на Python/FastAPI, а не на разных языках | Общее обоснование |
| 5 | Почему выбран Docker Compose вместо Kubernetes для оркестрации | `docker-compose.yml` |

**Шаблон ADR:**

```markdown
# ADR-001: <Тема решения>

## Статус
Принято

## Контекст
(Описание проблемы. Почему возникла необходимость в решении?
Какие ограничения существуют?)

## Решение
(Что именно сделано? Какая технология / паттерн выбраны?
Почему именно этот вариант?)

## Альтернативы
| Альтернатива | Плюсы | Минусы | Почему отклонена |
|---|---|---|---|
| ... | ... | ... | ... |

## Последствия
- (+) Положительные
- (−) Отрицательные / риски
- (!) На что обратить внимание при эксплуатации
```

**Контрольный вопрос:** Какие риски несёт использование Docker Compose в продакшене по сравнению с Kubernetes?

---

### Задание 6. Составление Runbook
**Форма:** самостоятельно | **Время:** 20 мин

**Цель:** создать операционную документацию для инженера эксплуатации.

**Порядок выполнения:**

1. Изучите `docker-compose.yml` выбранного сервиса.
2. Определите:
   - переменные окружения;
   - health-check эндпоинты;
   - порты и зависимости;
   - логи (где и как смотреть).
3. Составьте Runbook по шаблону:

```markdown
# Runbook: <название сервиса>

## Обзор
Назначение сервиса, критичность для бизнес-процесса.
Порт: <порт>
Health check: `GET /health`

## Зависимости
| Зависимость | Тип | Что произойдёт при отказе |
|---|---|---|
| PostgreSQL | Хранилище | Сервис не сможет создавать заказы |
| RabbitMQ | Брокер | События не будут опубликованы |
| Redis | Кэш | Увеличится время ответа, но сервис продолжит работу |

## Метрики и алерты
| Метрика | Порог алерта | Действие |
|---|---|---|
| `/health` возвращает 500 | > 1 мин | Перезапуск контейнера |
| Время ответа > 2 сек | > 5 мин | Проверить нагрузку на БД |
| Очередь в RabbitMQ > 1000 | > 3 мин | Проверить потребителей |

## Типовые инциденты

### Инцидент 1: Сервис не запускается
**Симптомы:** контейнер в состоянии `Restarting` или `Exited`
**Диагностика:**
```bash
docker-compose logs <имя_сервиса> --tail=100
docker-compose ps
```
**Возможные причины:**
- Неверные переменные окружения в `.env`
- PostgreSQL / RabbitMQ ещё не готовы (проблема старта)
**Решение:**
1. Проверить `.env` файл
2. Убедиться, что зависимости запущены: `docker-compose ps`
3. Перезапустить: `docker-compose restart <имя_сервиса>`

### Инцидент 2: События не доходят до потребителей
**Симптомы:** заказы создаются, но уведомления не приходят
**Диагностика:**
```bash
# Проверить очереди в RabbitMQ
docker exec rabbitmq rabbitmqctl list_queues

# Проверить логи потребителя
docker-compose logs notification-service --tail=50
```
**Решение:**
1. Проверить, что очередь существует и не переполнена
2. Проверить подключение потребителя к RabbitMQ
3. Проверить формат сообщения (соответствует ли схеме)

### Инцидент 3: Медленные ответы от product-service
**Симптомы:** время ответа > 2 сек
**Диагностика:**
```bash
# Проверить загрузку Redis
docker exec redis redis-cli info stats

# Проверить загрузку PostgreSQL
docker exec postgres psql -U user -d dbname -c "SELECT * FROM pg_stat_activity;"
```
**Решение:**
1. Проверить, что Redis запущен и доступен
2. Если кэш пуст — прогреть кэш
3. Проверить индексы в PostgreSQL

## Контакты эскалации
| Уровень | Роль | Канал |
|---|---|---|
| L1 | Дежурный инженер | #oncall |
| L2 | Команда сервиса | #team-... |
```

**Контрольный вопрос:** Что произойдёт, если `Redis` будет остановлен? Опишите пошагово, как это повлияет на `product-service` и на систему в целом.

---

## 5. Сводная таблица заданий

| № | Задание | Форма | Время | Результат |
|---|---|---|---|---|
| 1 | Аудит документации проекта | Аудиторно | 15 мин | Таблица аудита + список пробелов |
| 2 | README для сервиса | Аудиторно | 25 мин | Файл `README_STUDENT.md` |
| 3 | OpenAPI + AsyncAPI спецификации | Аудиторно | 20 мин | Файлы `openapi.yaml`, `asyncapi.yaml` |
| 4 | Диаграммы C4 (Mermaid) | Самостоятельно | 20 мин | Файлы `c4-context.mmd`, `c4-container.mmd` |
| 5 | Запись ADR | Самостоятельно | 20 мин | Файл `ADR-001.md` |
| 6 | Составление Runbook | Самостоятельно | 20 мин | Файл `RUNBOOK.md` |
| | **Итого** | | **~120 мин** | |

---

## 6. Требования к отчёту

Отчёт предоставляется в виде **одного архива** (`.zip`) или **ссылки на репозиторий** со следующей структурой:

```
practice-5-report/
├── 1-audit/
│   └── audit-table.md              # Задание 1
├── 2-readme/
│   └── README_STUDENT.md           # Задание 2
├── 3-api/
│   ├── openapi.yaml                # Задание 3а
│   └── asyncapi.yaml               # Задание 3б
├── 4-architecture/
│   ├── c4-context.mmd              # Задание 4а
│   └── c4-container.mmd            # Задание 4б
├── 5-adr/
│   └── ADR-001.md                  # Задание 5
├── 6-runbook/
│   └── RUNBOOK.md                  # Задание 6
└── README.md                       # Титульный лист: ФИО, группа, вариант
```

---

## 7. Критерии оценивания

| Критерий | Баллы | Описание |
|---|---|---|
| Полнота выполнения всех 6 заданий | 0–20 | По 3 балла за задание + 2 за оформление |
| Корректность извлечения информации из кода | 0–20 | Правильность описания роутов, событий, конфигурации |
| Соответствие шаблонам (README, ADR, Runbook) | 0–15 | Все обязательные разделы заполнены |
| Качество диаграмм C4 | 0–15 | Полнота, корректность связей, читаемость |
| Обоснованность выводов (пробелы, ADR) | 0–15 | Аргументация, конкретность |
| Ответы на контрольные вопросы | 0–15 | Полнота и точность ответов |
| **Итого** | **100** | |

### Шкала перевода

| Баллы | Оценка |
|---|---|
| 90–100 | Отлично (5) |
| 75–89 | Хорошо (4) |
| 60–74 | Удовлетворительно (3) |
| 0–59 | Неудовлетворительно (2) |

---

## 8. Рекомендуемые источники

1. Лекция 6: «Документирование систем с МСА».
2. Документация FastAPI: [fastapi.tiangolo.com](https://fastapi.tiangolo.com/)
3. OpenAPI Specification: [spec.openapis.org](https://spec.openapis.org/oas/v3.1.0)
4. AsyncAPI Specification: [asyncapi.com/docs](https://www.asyncapi.com/docs)
5. RabbitMQ Documentation: [rabbitmq.com/documentation](https://www.rabbitmq.com/documentation.html)
6. Redis Documentation: [redis.io/docs](https://redis.io/docs/)
7. Diátaxis Framework: [diataxis.fr](https://diataxis.fr/)
8. C4 Model: [c4model.com](https://c4model.com/)
9. Mermaid Syntax: [mermaid.js.org](https://mermaid.js.org/)
10. Docker Compose Documentation: [docs.docker.com/compose](https://docs.docker.com/compose/)

---

