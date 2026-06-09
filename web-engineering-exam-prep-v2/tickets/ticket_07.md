# Билет №7. gRPC, REST, Protocol Buffers

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Сравните gRPC и REST: протокол, формат данных, производительность, типизация. Почему для backend CRM выбран gRPC?

### Теоретический вопрос 2

Что такое Protocol Buffers (protobuf)? Какие типы вызовов поддерживает gRPC (unary, server streaming, client streaming, bidirectional)?

### Практический вопрос

Напишите файл user.proto с сообщениями UserRequest, UserResponse и сервисом UserService с методами GetUser и CreateUser.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**REST** — архитектурный стиль, чаще всего использующий HTTP и JSON. Ресурсы доступны через URL, например `/users/1`, а операции выражаются HTTP-методами: GET, POST, PUT, DELETE.

**gRPC** — RPC-фреймворк, где клиент вызывает методы сервиса почти как обычные функции. Обычно использует HTTP/2 и Protocol Buffers.

| Критерий | REST | gRPC |
|---|---|---|
| Стиль | работа с ресурсами | вызов методов сервиса |
| Протокол | обычно HTTP/1.1 или HTTP/2 | HTTP/2 |
| Формат | чаще JSON | Protocol Buffers |
| Типизация | слабее, через документацию/OpenAPI | строгий контракт `.proto` |
| Производительность | хорошая, но JSON тяжелее | высокая, бинарный формат |
| Streaming | не основной сценарий | поддерживается встроенно |

Для backend CRM выбран gRPC, потому что он даёт строгий контракт, хорошую производительность и удобен для взаимодействия сервисов внутри кластера.

#### Как лучше ответить преподавателю

REST обычно использует HTTP и JSON, он проще и удобен для публичных API. gRPC использует контракт protobuf, бинарную сериализацию и HTTP/2, поэтому он быстрее и строже типизирован. Для внутреннего backend CRM gRPC удобен из-за контракта, производительности и автогенерации клиентов/серверов.

### Теоретический вопрос 2

#### Пояснение

**Protocol Buffers** — это формат описания сообщений и сервисов. В `.proto`-файле задаются структуры данных и методы gRPC-сервиса. По этому файлу можно сгенерировать клиентский и серверный код.

gRPC поддерживает 4 типа вызовов:

1. **Unary** — один запрос, один ответ.
2. **Server streaming** — один запрос, поток ответов от сервера.
3. **Client streaming** — поток запросов от клиента, один ответ сервера.
4. **Bidirectional streaming** — поток запросов и поток ответов одновременно.

#### Как лучше ответить преподавателю

Protocol Buffers — это формат описания сообщений и сервисов для gRPC. В `.proto` файле задаются структуры данных и RPC-методы. gRPC поддерживает unary-вызов, server streaming, client streaming и bidirectional streaming.

---

## 3. Практика

### Что важно показать

В `.proto` важно указать syntax, package, сообщения request/response и service с RPC-методами.

### Готовое решение

`user.proto`:

```proto
syntax = "proto3";

package crm.user;

service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
  rpc CreateUser (CreateUserRequest) returns (UserResponse);
}

message UserRequest {
  string id = 1;
}

message CreateUserRequest {
  string email = 1;
  string first_name = 2;
  string last_name = 3;
}

message UserResponse {
  string id = 1;
  string email = 2;
  string first_name = 3;
  string last_name = 4;
  string created_at = 5;
}
```

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
