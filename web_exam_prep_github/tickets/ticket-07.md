# Билет №7 — gRPC, REST, protobuf, user.proto

## 0. Как понять этот билет простыми словами

Билет про общение клиента и сервера.

```text
REST = HTTP + JSON + ресурсы
gRPC = HTTP/2 + protobuf + методы
```

---

## 1. Теоретический вопрос 1

**Вопрос:** Сравните gRPC и REST: протокол, формат данных, производительность, типизация. Почему для backend CRM выбран gRPC?

### Простое объяснение

REST обычно использует HTTP и JSON, работает с ресурсами. gRPC работает поверх HTTP/2, использует Protocol Buffers и описывает API как методы.

gRPC быстрее и строже типизирован, удобен для внутренних сервисов CRM.

### Готовый ответ на 1 вопрос

REST и gRPC — это подходы к взаимодействию клиента и сервера. REST обычно использует HTTP и JSON, работает с ресурсами и прост в понимании. gRPC работает поверх HTTP/2, использует Protocol Buffers и описывает API как набор удалённых методов.

gRPC обычно быстрее за счёт бинарного формата protobuf и HTTP/2. Также он обеспечивает строгую типизацию через `.proto`-файлы, из которых можно генерировать клиентский и серверный код. Для backend CRM выбран gRPC, потому что система имеет много внутренних вызовов, требует понятного контракта, типизации и эффективного обмена данными между сервисами.

---

## 2. Теоретический вопрос 2

**Вопрос:** Что такое Protocol Buffers? Какие типы вызовов поддерживает gRPC?

### Простое объяснение

Protocol Buffers — формат описания сообщений и сервисов в `.proto`.

Типы вызовов:

```text
unary: один запрос → один ответ
server streaming: один запрос → поток ответов
client streaming: поток запросов → один ответ
bidirectional: поток ↔ поток
```

### Готовый ответ на 2 вопрос

Protocol Buffers — это механизм сериализации данных и язык описания контрактов gRPC. В `.proto`-файле описываются сообщения, их поля и сервисы с методами. На основе этого файла можно сгенерировать код клиента и сервера.

gRPC поддерживает четыре типа вызовов: unary, server streaming, client streaming и bidirectional streaming.

---

## 3. Практическое задание

**Задание:** Напишите файл `user.proto` с сообщениями `UserRequest`, `UserResponse` и сервисом `UserService`.

### Решение

```proto
syntax = "proto3";

package users;

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

## 4. Краткая версия для устного пересказа

REST — HTTP и JSON, gRPC — HTTP/2 и protobuf. gRPC быстрее, типизированнее и удобнее для внутренних сервисов CRM. Protobuf описывает сообщения и сервисы. gRPC поддерживает unary, server streaming, client streaming и bidirectional streaming.

---

## 5. Что обязательно сказать

- REST = HTTP + JSON + ресурсы.
- gRPC = HTTP/2 + protobuf + методы.
- Protobuf даёт контракт.
- gRPC удобен для CRM backend.
- Есть 4 типа вызовов.

---

## 6. Типичные ошибки

1. Сказать, что gRPC всегда заменяет REST.
2. Забыть HTTP/2.
3. Не назвать 4 типа вызовов.
4. Написать proto без syntax.
5. Не объяснить типизацию.

---

## 7. Мини-тренировка

1. Чем gRPC отличается от REST?
2. Что такое protobuf?
3. Что такое unary?
4. Что такое server streaming?
5. Почему CRM выбирает gRPC?
