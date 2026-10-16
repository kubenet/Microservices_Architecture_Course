# Список из 10 открытых проектов для практических работ по МСА

## Сводная таблица проектов

| № | Проект | Стек технологий | Кол-во сервисов | Сложность | Паттерны |
|---|---|---|---|---|---|
| 1 | **[FastAPI Microservices Demo](https://github.com/tiangolo/full-stack-fastapi-template)** | Python, FastAPI, PostgreSQL, Redis | 3–5 | ⭐ Легкая | REST, Docker Compose |
| 2 | **[Sock Shop](https://github.com/microservices-demo/microservices-demo)** | Java, Go, Node.js, MongoDB, RabbitMQ | 8 | ⭐⭐ Средняя | REST, AMQP, Kubernetes |
| 3 | **[Robot Shop](https://github.com/instana/robot-shop)** | Python, Go, Java, Node.js, PHP, MongoDB | 8 | ⭐⭐ Средняя | REST, gRPC, OpenTelemetry |
| 4 | **[eShop on Containers](https://github.com/dotnet/eShop)** | .NET, C#, RabbitMQ, PostgreSQL, Redis | 10+ | ⭐⭐⭐ Средняя+ | CQRS, Event Bus, DDD |
| 5 | **[Online Boutique (Google)](https://github.com/GoogleCloudPlatform/microservices-demo)** | Go, Python, Java, Node.js, gRPC | 11 | ⭐⭐⭐ Средняя+ | gRPC, Kubernetes, Istio |
| 6 | **[Train Ticket (Fudan)](https://github.com/FudanSELab/train-ticket)** | Java, Spring Boot, MongoDB, RabbitMQ | 40+ | ⭐⭐⭐⭐ Высокая | REST, Saga, Distributed Tracing |
| 7 | **[Coolstore](https://github.com/vietnam-devs/coolstore-microservices)** | .NET, gRPC, GraphQL, RabbitMQ | 7 | ⭐⭐⭐ Средняя+ | CQRS, Event Sourcing, DDD |
| 8 | **[FTGO](https://github.com/ftgo/ftgo-application)** | Java, Spring Boot, Kafka, MySQL | 6 | ⭐⭐⭐⭐ Высокая | Saga, Event Sourcing, DDD |
| 9 | **[Media Microservices (Cornell)](https://github.com/dgryski/mediamicroservices)** | Go, C++, MongoDB, Redis, Kafka | 15+ | ⭐⭐⭐⭐⭐ Очень высокая | gRPC, Thrift, Social Network |
| 10 | **[DeathStar Bench (Stanford)](https://github.com/delimitrou/DeathStarBench)** | C++, Go, Python, MongoDB, Redis | 20+ | ⭐⭐⭐⭐⭐ Очень высокая | gRPC, Thrift, High Load |

---

## Подробное описание проектов

### 1. FastAPI Microservices Demo
**Ссылка:** https://github.com/tiangolo/full-stack-fastapi-template

**Описание:** Шаблон полнофункционального приложения от создателя FastAPI Sebastián Ramírez. Простая архитектура с аутентификацией, CRUD-операциями и админ-панелью.

**Стек:**
- Backend: Python 3.11+, FastAPI, SQLAlchemy
- Frontend: React
- БД: PostgreSQL
- Кэш: Redis
- Оркестрация: Docker Compose

**Сервисы:**
- `backend` (API)
- `frontend` (UI)
- `db` (PostgreSQL)
- `redis` (кэш)

**Для каких практических работ подходит:**
- Практика №1: Введение в МСА (анализ простой архитектуры)
- Практика №5: Документирование (базовый уровень)
- Курсовая работа: Реализация собственного микросервиса

**Преимущества:**
- Минимальная сложность для начинающих
- Отличная документация FastAPI
- Современный стек Python
- Активная поддержка

**Недостатки:**
- Фактически монолит с элементами микросервисов
- Нет асинхронного взаимодействия между сервисами

---

### 2. Sock Shop
**Ссылка:** https://github.com/microservices-demo/microservices-demo

**Описание:** Классический демонстрационный проект интернет-магазина носков. Один из первых эталонных проектов для обучения МСА.

**Стек:**
- Языки: Java (Spring Boot), Go, Node.js
- БД: MongoDB, MySQL
- Брокер: RabbitMQ
- Оркестрация: Kubernetes, Docker Compose

**Сервисы:**
- `front-end` (UI)
- `orders` (заказы)
- `catalogue` (каталог)
- `carts` (корзина)
- `user` (пользователи)
- `payment` (платежи)
- `shipping` (доставка)
- `queue-master` (обработка очередей)

**Для каких практических работ подходит:**
- Практика №2: Анализ архитектуры существующей системы
- Практика №3: Паттерны взаимодействия сервисов
- Практика №5: Документирование
- Практика №6: Миграция (если добавить монолитную версию)

**Преимущества:**
- Разнообразие языков программирования
- Хорошая документация
- Множество вариантов развёртывания (Docker, K8s, AWS, Azure)
- Широко используется в обучении

**Недостатки:**
- Устаревший стек (не обновляется активно с 2020 года)
- Упрощённая бизнес-логика

---

### 3. Robot Shop
**Ссылка:** https://github.com/instana/robot-shop

**Описание:** Интернет-магазин роботов, созданный компанией Instana для демонстрации возможностей мониторинга и трассировки в микросервисных системах.

**Стек:**
- Языки: Python, Go, Java, Node.js, PHP
- БД: MongoDB, MySQL
- Брокер: RabbitMQ
- Мониторинг: OpenTelemetry, Jaeger
- Оркестрация: Kubernetes

**Сервисы:**
- `web` (UI)
- `cart` (корзина)
- `catalogue` (каталог)
- `payment` (платежи)
- `shipping` (доставка)
- `user` (пользователи)
- `ratings` (рейтинги)
- `load` (генератор нагрузки)

**Для каких практических работ подходит:**
- Практика №3: Паттерны взаимодействия
- Практика №4: Мониторинг и наблюдаемость (Observability)
- Практика №5: Документирование
- Лабораторная работа: Трассировка запросов

**Преимущества:**
- Фокус на Observability (метрики, логи, трассы)
- Разнообразие языков
- Встроенная генерация нагрузки
- Отличная база для изучения OpenTelemetry

**Недостатки:**
- Сложная настройка мониторинга
- Требует Kubernetes для полноценного развёртывания

---

### 4. eShop on Containers (.NET eShop)
**Ссылка:** https://github.com/dotnet/eShop

**Описание:** Эталонное приложение от Microsoft, демонстрирующее современные паттерны МСА на платформе .NET. Реализует сложные архитектурные паттерны.

**Стек:**
- Язык: C# (.NET 8)
- Фреймворк: ASP.NET Core
- БД: PostgreSQL, MongoDB, Redis
- Брокер: RabbitMQ
- Оркестрация: Docker, Kubernetes
- API Gateway: YARP

**Сервисы:**
- `basket-api` (корзина)
- `catalog-api` (каталог)
- `ordering-api` (заказы)
- `identity-api` (аутентификация)
- `payment-api` (платежи)
- `webhooks-api` (вебхуки)
- `ordering-backgroundservice` (фоновые задачи)
- `aggregator-api` (агрегация)

**Паттерны:**
- CQRS (Command Query Responsibility Segregation)
- Event Sourcing
- Event Bus (RabbitMQ)
- DDD (Domain-Driven Design)
- API Gateway

**Для каких практических работ подходит:**
- Практика №3: Паттерны взаимодействия (CQRS, Event Sourcing)
- Практика №4: Мониторинг
- Практика №5: Документирование сложных систем
- Курсовая работа: Реализация паттерна Saga

**Преимущества:**
- Современный стек .NET 8
- Глубокая реализация DDD и CQRS
- Отличная документация от Microsoft
- Активная поддержка

**Недостатки:**
- Высокий порог входа (требуется знание .NET)
- Сложность бизнес-логики

---

### 5. Online Boutique (Google Microservices Demo)
**Ссылка:** https://github.com/GoogleCloudPlatform/microservices-demo

**Описание:** Демонстрационное приложение интернет-магазина от Google Cloud Platform. Используется для демонстрации возможностей GKE (Google Kubernetes Engine) и Istio.

**Стек:**
- Языки: Go, Python, Java, Node.js, C#, Ruby
- БД: PostgreSQL (через gRPC)
- Протокол: gRPC
- Service Mesh: Istio
- Оркестрация: Kubernetes

**Сервисы:**
- `frontend` (UI)
- `cartservice` (корзина)
- `productcatalogservice` (каталог)
- `currencyservice` (валюты)
- `paymentservice` (платежи)
- `shippingservice` (доставка)
- `emailservice` (email)
- `checkoutservice` (оформление заказа)
- `recommendationservice` (рекомендации)
- `adservice` (реклама)
- `loadgenerator` (генератор нагрузки)

**Для каких практических работ подходит:**
- Практика №2: Анализ архитектуры
- Практика №3: Паттерны взаимодействия (gRPC)
- Практика №5: Документирование
- Лабораторная работа: Service Mesh (Istio)

**Преимущества:**
- Эталонный проект от Google
- Разнообразие языков
- Отличная документация
- Интеграция с облачными сервисами GCP

**Недостатки:**
- Фокус на gRPC (нет REST API)
- Требует Kubernetes
- Сложность настройки Istio

---

### 6. Train Ticket (Fudan University)
**Ссылка:** https://github.com/FudanSELab/train-ticket

**Описание:** Крупномасштабная система бронирования билетов на поезда, разработанная в Фуданьском университете. Один из крупнейших открытых МСА-проектов.

**Стек:**
- Язык: Java (Spring Boot)
- БД: MongoDB, MySQL
- Брокер: RabbitMQ
- Трассировка: SkyWalking, Jaeger
- Оркестрация: Kubernetes

**Сервисы (40+):**
- `ts-ui-dashboard` (UI)
- `ts-auth-service` (аутентификация)
- `ts-user-service` (пользователи)
- `ts-contacts-service` (контакты)
- `ts-order-service` (заказы)
- `ts-order-other-service` (другие заказы)
- `ts-config-service` (конфигурация)
- `ts-station-service` (станции)
- `ts-train-service` (поезда)
- `ts-route-service` (маршруты)
- `ts-travel-service` (путешествия)
- `ts-travel2-service` (путешествия v2)
- `ts-preserve-service` (бронирование)
- `ts-preserve-other-service` (другое бронирование)
- `ts-basic-service` (базовый)
- `ts-ticketinfo-service` (информация о билетах)
- `ts-price-service` (цены)
- `ts-notification-service` (уведомления)
- `ts-security-service` (безопасность)
- `ts-inside-payment-service` (внутренние платежи)
- `ts-payment-service` (платежи)
- `ts-rebook-service` (перебронирование)
- `ts-cancel-service` (отмена)
- `ts-food-map-service` (еда)
- `ts-food-service` (заказ еды)
- `ts-consign-service` (багаж)
- `ts-consign-price-service` (цена багажа)
- `ts-execute-service` (исполнение)
- `ts-admin-basic-info-service` (админ)
- `ts-admin-order-service` (админ заказы)
- `ts-admin-route-service` (админ маршруты)
- `ts-admin-travel-service` (админ путешествия)
- `ts-admin-user-service` (админ пользователи)
- `ts-delivery-service` (доставка)
- `ts-wait-order-service` (ожидание заказов)
- `ts-assurance-service` (страхование)
- `ts-seat-service` (места)
- `ts-travel-plan-service` (план путешествия)
- `ts-ticket-office-service` (касса)
- `ts-news-service` (новости)
- `ts-voucher-service` (ваучеры)

**Для каких практических работ подходит:**
- Практика №3: Паттерны взаимодействия (Saga, оркестрация)
- Практика №4: Мониторинг крупномасштабных систем
- Практика №6: Миграция и рефакторинг
- Дипломная работа: Анализ производительности

**Преимущества:**
- Огромное количество сервисов (реалистичная enterprise-система)
- Реализация паттерна Saga
- Встроенная трассировка
- Научная база (исследования Фуданьского университета)

**Недостатки:**
- Очень высокая сложность
- Требует значительных ресурсов для запуска
- Слабая документация на английском

---

### 7. Coolstore
**Ссылка:** https://github.com/vietnam-devs/coolstore-microservices

**Описание:** Интернет-магазин электроники, демонстрирующий современные паттерны МСА на платформе .NET с использованием gRPC и GraphQL.

**Стек:**
- Язык: C# (.NET 8)
- API: gRPC, GraphQL, REST
- БД: PostgreSQL, MongoDB, Redis
- Брокер: RabbitMQ
- Оркестрация: Docker, Kubernetes

**Сервисы:**
- `product-catalog-service` (каталог)
- `inventory-service` (инвентарь)
- `cart-service` (корзина)
- `rating-service` (рейтинги)
- `shipping-service` (доставка)
- `payment-service` (платежи)
- `identity-service` (аутентификация)

**Паттерны:**
- CQRS
- Event Sourcing
- Saga (оркестрируемая)
- API Composition
- GraphQL Federation

**Для каких практических работ подходит:**
- Практика №3: Паттерны взаимодействия (gRPC, GraphQL)
- Практика №4: Мониторинг
- Курсовая работа: Реализация Saga

**Преимущества:**
- Современный стек .NET 8
- Разнообразие API (gRPC, GraphQL, REST)
- Хорошая реализация Saga
- Активная поддержка

**Недостатки:**
- Требует знания .NET
- Сложность настройки GraphQL Federation

---

### 8. FTGO (Food Delivery Application)
**Ссылка:** https://github.com/ftgo/ftgo-application

**Описание:** Приложение доставки еды, описанное в книге Криса Ричардсона "Microservices Patterns". Реализует сложные паттерны МСА.

**Стек:**
- Язык: Java (Spring Boot)
- БД: MySQL
- Брокер: Apache Kafka
- Оркестрация: Docker

**Сервисы:**
- `consumer-service` (потребители)
- `restaurant-service` (рестораны)
- `order-service` (заказы)
- `kitchen-service` (кухня)
- `accounting-service` (бухгалтерия)
- `delivery-service` (доставка)

**Паттерны:**
- Saga (хореография и оркестрация)
- Event Sourcing
- CQRS
- API Composition
- Transactional Outbox

**Для каких практических работ подходит:**
- Практика №3: Паттерны взаимодействия (Saga, Event Sourcing)
- Практика №5: Документирование
- Курсовая работа: Реализация распределённых транзакций

**Преимущества:**
- Эталонная реализация паттернов из книги Ричардсона
- Глубокая проработка бизнес-логики
- Отличная база для изучения Saga

**Недостатки:**
- Требует знания Java и Kafka
- Устаревший стек (Spring Boot 2.x)
- Слабая документация

---

### 9. Media Microservices (Cornell University)
**Ссылка:** https://github.com/dgryski/mediamicroservices

**Описание:** Социальная сеть для обмена медиа-контентом, разработанная в Корнеллском университете для исследований производительности МСА.

**Стек:**
- Языки: Go, C++, Python
- БД: MongoDB, Redis
- Брокер: Apache Kafka
- Протоколы: gRPC, Thrift
- Оркестрация: Kubernetes

**Сервисы (15+):**
- `nginx-web-server` (веб-сервер)
- `media-frontend` (фронтенд)
- `user-service` (пользователи)
- `user-timeline-service` (лента)
- `post-storage-service` (хранение постов)
- `home-timeline-service` (домашняя лента)
- `user-mention-service` (упоминания)
- `url-shorten-service` (короткие ссылки)
- `media-service` (медиа)
- `text-service` (текст)
- `unique-id-service` (уникальные ID)
- `social-graph-service` (социальный граф)
- `recommendation-service` (рекомендации)
- `compose-post-service` (создание постов)
- `social-network-service` (социальная сеть)

**Для каких практических работ подходит:**
- Практика №4: Мониторинг и производительность
- Лабораторная работа: Анализ задержек
- Дипломная работа: Оптимизация МСА

**Преимущества:**
- Высокая производительность
- Реалистичная нагрузка
- Научная база (исследования Корнелла)
- Разнообразие протоколов (gRPC, Thrift)

**Недостатки:**
- Очень высокая сложность
- Требует значительных ресурсов
- Слабая документация

---

### 10. DeathStar Bench (Stanford University)
**Ссылка:** https://github.com/delimitrou/DeathStarBench

**Описание:** Набор бенчмарков для исследования производительности МСА, разработанный в Стэнфордском университете. Включает несколько приложений разной сложности.

**Приложения:**
1. **Social Network** (социальная сеть)
2. **Media Service** (медиа-сервис)
3. **E-commerce** (электронная коммерция)
4. **Banking System** (банковская система)
5. **Hotel Reservation** (бронирование отелей)

**Стек:**
- Языки: C++, Go, Python, Lua
- БД: MongoDB, Redis, Memcached
- Брокер: Kafka, RabbitMQ
- Протоколы: gRPC, Thrift, HTTP
- Оркестрация: Kubernetes

**Для каких практических работ подходит:**
- Практика №4: Мониторинг и производительность
- Лабораторная работа: Стресс-тестирование
- Дипломная работа: Сравнительный анализ производительности

**Преимущества:**
- Научная база (исследования Стэнфорда)
- Разнообразие приложений
- Фокус на производительности
- Реалистичные сценарии нагрузки

**Недостатки:**
- Очень высокая сложность
- Требует Kubernetes и значительных ресурсов
- Ориентирован на исследования, а не на обучение

---

## Рекомендации по использованию

### Для начинающих (курсы 3–4 бакалавриата)
**Рекомендуемые проекты:** 1, 2, 3

- **FastAPI Microservices Demo** — для первых практических работ
- **Sock Shop** — для анализа классической МСА
- **Robot Shop** — для изучения Observability

**Практические работы:**
- Практика №1: Введение в МСА
- Практика №2: Анализ архитектуры
- Практика №5: Документирование

### Для продолжающих (магистратура, специалитет)
**Рекомендуемые проекты:** 4, 5, 6, 7

- **eShop** — для изучения CQRS и Event Sourcing
- **Online Boutique** — для изучения gRPC и Service Mesh
- **Train Ticket** — для анализа крупномасштабных систем
- **Coolstore** — для изучения Saga

**Практические работы:**
- Практика №3: Паттерны взаимодействия
- Практика №4: Мониторинг
- Практика №6: Миграция

### Для продвинутых (дипломные работы, исследования)
**Рекомендуемые проекты:** 8, 9, 10

- **FTGO** — для реализации распределённых транзакций
- **Media Microservices** — для анализа производительности
- **DeathStar Bench** — для исследований и бенчмаркинга

**Работы:**
- Курсовые проекты
- Дипломные работы
- Научные исследования

---

## Соответствие темам курса

| Тема курса | Рекомендуемые проекты |
|---|---|
| Лекция 1: Введение в МСА | 1, 2, 3 |
| Лекция 2: Архитектурные стили | 2, 3, 5 |
| Лекция 3: Паттерны взаимодействия | 4, 5, 6, 7, 8 |
| Лекция 4: Управление данными | 4, 7, 8 |
| Лекция 5: Развёртывание и оркестрация | 2, 3, 5, 6, 9, 10 |
| Лекция 6: Документирование | Все проекты |
| Лекция 7: Миграция | 2, 4, 6 |
| Лекция 8: Мониторинг и Observability | 3, 5, 9, 10 |
| Лекция 9: Безопасность | 4, 5, 7 |
| Лекция 10: Производительность | 9, 10 |

---

## Дополнительные ресурсы

### Где искать больше проектов
- **GitHub Topics:** https://github.com/topics/microservices
- **Awesome Microservices:** https://github.com/mfornos/awesome-microservices
- **Microsoft eShop:** https://github.com/dotnet/eShop
- **Google Cloud Examples:** https://github.com/GoogleCloudPlatform

### Инструменты для анализа проектов
- **ArchiMate:** для моделирования архитектуры
- **Structurizr:** для создания C4-диаграмм
- **Mermaid:** для текстовых диаграмм
- **Docker Scout:** для анализа Docker-образов
- **SonarQube:** для анализа качества кода

### Метрики для сравнения проектов
- Количество сервисов
- Разнообразие языков программирования
- Сложность бизнес-логики
- Наличие паттернов (Saga, CQRS, Event Sourcing)
- Качество документации
- Активность поддержки (commits, issues, PRs)
- Размер Docker-образов
- Время запуска
- Требования к ресурсам

---

*Список составлен на основе анализа открытых проектов на GitHub и рекомендаций из рабочей программы дисциплины "Микросервисная архитектура".*