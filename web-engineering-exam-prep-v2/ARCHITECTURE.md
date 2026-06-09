# Общая архитектура из roadmap

[← Назад к списку билетов](README.md)

В курсе рассматривается WEB-приложение на базе CRM-кластера с паттерном **Publisher/Subscriber**.

```mermaid
flowchart LR
    WebClient[Web client: HTML/CSS/TS] --> GrpcWrap[Обёртка над gRPC]
    GrpcWrap --> BackendAPI[Backend gRPC API]
    BackendAPI --> Service[Service / Logic]
    Service --> Repo[Repository]
    Repo --> Postgres[(PostgreSQL)]
    Repo --> SQLite[(SQLite)]
    Repo --> SQLServer[(SQL Server)]
    BackendAPI --> Redis[(Redis)]
    BackendAPI --> Fias[Fias API / Города]
    Publisher[Издатель] --> SubSvc[Сервис подписок]
    SubSvc --> Kafka[Kafka]
    Kafka --> RabbitMQ[RabbitMQ / очереди]
    RabbitMQ --> Subscribers[Подписчики 1/2/3]
    Service --> Audit[Log / Audit]
```

Главная логика курса: клиент не работает с БД напрямую. Он обращается к backend через gRPC, backend вызывает бизнес-логику и Repository, а асинхронные события идут через Kafka/RabbitMQ к подписчикам.
