# Билет №2. Subscriptions, Kafka, RabbitMQ, SQL-схема

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Что представляет собой сущность Subscriptions в CRM-системе? Какие поля и связи должна содержать модель подписки?

### Теоретический вопрос 2

Как сервис подписок взаимодействует с Kafka и очередями сообщений? Объясните, зачем нужны RabbitMQ и Kafka в crm-cluster и чем они отличаются по назначению.

### Практический вопрос

Спроектируйте SQL-схему таблицы subscriptions (PostgreSQL): первичный ключ, внешние ключи на User, статус подписки, дата создания, тип подписчика (1/2/3). Приведите DDL-запрос CREATE TABLE.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**Subscriptions** — это сущность подписки в CRM-системе. Она показывает, какой пользователь на что подписан, в каком статусе находится подписка и какой тип подписчика используется.

Модель подписки обычно содержит:

- `id` — уникальный идентификатор подписки;
- `user_id` — связь с пользователем `User`;
- `subscriber_type` — тип подписчика: 1, 2 или 3;
- `status` — статус: active, cancelled, pending, expired;
- `created_at` — дата создания;
- `updated_at` — дата изменения;
- `cancelled_at` — дата отмены, если подписка отменена;
- `source` — источник подписки, например web, admin, api;
- связи с платежами, событиями, аудитом.

Главная связь: один `User` может иметь много `Subscriptions`.

#### Как лучше ответить преподавателю

Subscriptions — это доменная сущность подписки: она связывает пользователя, тип подписчика, статус и даты жизненного цикла. Минимально в ней должны быть id, user_id, subscriber_type, status, created_at и updated_at; дополнительно могут быть cancelled_at, source и связи с платежами/аудитом.

### Теоретический вопрос 2

#### Пояснение

**Сервис подписок** отвечает за создание, изменение и отмену подписок. При важных изменениях он публикует события в брокер сообщений.

**Kafka** и **RabbitMQ** нужны для асинхронной обработки сообщений.

| Критерий | Kafka | RabbitMQ |
|---|---|---|
| Основная идея | распределённый лог событий | брокер очередей сообщений |
| Лучше подходит для | потоков событий, аналитики, event streaming | задач, команд, очередей обработки |
| Хранение сообщений | хранит события определённое время | обычно сообщение исчезает после обработки |
| Модель | topic + consumer groups | exchange + queue + routing |
| Пример в CRM | поток событий подписок | очередь задач для control-rule manager |

В crm-cluster Kafka удобно использовать как шину событий: «подписка создана», «платёж создан», «статус изменён». RabbitMQ удобно использовать для задач, которые нужно гарантированно выполнить конкретными обработчиками.

#### Как лучше ответить преподавателю

Сервис подписок при изменении подписки публикует событие в брокер. Kafka удобна как поток событий и журнал для многих подписчиков, RabbitMQ — как очередь задач с маршрутизацией, retry и dead-letter. В crm-cluster они помогают развязать сервисы и не выполнять все операции синхронно.

---

## 3. Практика

### Что важно показать

В DDL нужно показать первичный ключ, связь с пользователем, ограничение типа подписчика и статус жизненного цикла подписки.

### Готовое решение

DDL PostgreSQL:

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE
);

CREATE TABLE subscriptions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL,
    subscriber_type SMALLINT NOT NULL,
    status VARCHAR(30) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_subscriptions_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT chk_subscriber_type
        CHECK (subscriber_type IN (1, 2, 3)),

    CONSTRAINT chk_subscription_status
        CHECK (status IN ('pending', 'active', 'cancelled', 'expired'))
);

CREATE INDEX idx_subscriptions_user_id ON subscriptions(user_id);
CREATE INDEX idx_subscriptions_status ON subscriptions(status);
```

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
