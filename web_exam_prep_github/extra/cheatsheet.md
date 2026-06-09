# Мини-шпаргалка по всем билетам

## Самые важные определения

```text
ФТ = что система должна делать.
pub/sub = издатель публикует событие, подписчики получают.
Subscriptions = сущность подписки пользователя.
Web client = клиентская часть в браузере.
HTML = структура.
CSS = оформление.
TypeScript/JavaScript = логика.
Component = самостоятельная часть интерфейса.
Input = данные внутрь компонента.
Output = событие наружу.
gRPC = быстрый типизированный RPC поверх HTTP/2.
protobuf = контракт сообщений и сервисов для gRPC.
DTO = объект передачи данных.
Entity = внутренняя доменная сущность.
Repository = слой доступа к данным.
Redis = быстрый кэш в памяти.
Fias API = внешний справочник адресов.
Assessment = модуль оценки/начисления.
Logic = бизнес-правила.
Save-1 = операция сохранения результата.
Log = технический журнал.
Audit = журнал значимых действий.
Идентификация = кто ты.
Аутентификация = докажи, что это ты.
Авторизация = что тебе можно.
ACL = список прав доступа.
ABAC = права по атрибутам.
REBAC = права по отношениям.
Deployment = запуск pod'ов в Kubernetes.
Service = стабильный доступ к pod'ам.
Ingress = внешний входной трафик.
```

## Главные цепочки

### Pub/sub

```text
Издатель → Сервис подписок → Kafka/очередь → Подписчики 1/2/3
```

### Backend-вызов

```text
Web client → gRPC wrapper → backend gRPC API → service → repository → PostgreSQL
```

### Login

```text
email/login → password → 2FA → JWT/session token → проверка прав
```

### Git workflow

```text
main → feature branch → commits → push → PR/MR → review → merge → tag
```

### Kubernetes

```text
User → Ingress → Service → Deployment/Pods → backend → Postgres/Redis
```
