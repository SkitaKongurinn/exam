# Билет №9 — CRUD через backend gRPC API, CreateUser

## 0. Как понять этот билет простыми словами

CRUD — четыре базовые операции:

```text
Create = создать
Read = прочитать
Update = обновить
Delete = удалить
```

Клиент не ходит напрямую в PostgreSQL. Он идёт через backend gRPC API.

---

## 1. Теоретический вопрос 1

**Вопрос:** Как реализовать CRUD-операции через backend grpc API? Опишите методы Create, Read, Update, Delete для User.

### Простое объяснение

Для User методы: `CreateUser`, `GetUser`, `UpdateUser`, `DeleteUser`.

Цепочка:

```text
gRPC request → transport → service → repository → database → response
```

### Готовый ответ на 1 вопрос

CRUD-операции через backend gRPC API реализуются как набор методов сервиса для сущности User. `CreateUser` принимает данные нового пользователя, валидирует их и сохраняет через repository в PostgreSQL. `GetUser` получает пользователя по id. `UpdateUser` проверяет существование пользователя и обновляет его поля. `DeleteUser` удаляет пользователя или помечает его как удалённого.

Все методы должны проходить через backend: transport-слой принимает gRPC-запрос, service выполняет бизнес-логику и валидацию, repository работает с базой. Клиент получает DTO пользователя.

---

## 2. Теоретический вопрос 2

**Вопрос:** Сравните прямую работу Web client с PostgreSQL и работу через gRPC. Почему прямой доступ к БД с клиента недопустим?

### Простое объяснение

Плохой вариант:

```text
Web client → PostgreSQL
```

Правильный вариант:

```text
Web client → backend gRPC API → Repository → PostgreSQL
```

Прямой доступ опасен: нужно раскрывать доступы к БД, нельзя нормально проверять права, можно обойти бизнес-логику, сложно логировать и аудитировать.

### Готовый ответ на 2 вопрос

Прямая работа Web client с PostgreSQL недопустима, потому что клиентское приложение работает в браузере и не является доверенной средой. Если дать ему прямой доступ к базе данных, придётся раскрывать данные подключения, что создаёт серьёзную угрозу безопасности.

Кроме того, при прямом доступе клиент может обойти бизнес-логику, проверки прав, валидацию, логирование и аудит. Правильный подход — работа через backend gRPC API. Backend проверяет пользователя, выполняет авторизацию, применяет бизнес-правила, обращается к базе через Repository и возвращает клиенту безопасный DTO.

---

## 3. Практическое задание

**Задание:** Напишите псевдокод gRPC-метода `CreateUser`.

### Решение

```ts
interface CreateUserRequest {
    email: string;
    firstName: string;
    lastName: string;
}

interface UserDto {
    id: string;
    email: string;
    firstName: string;
    lastName: string;
    createdAt: string;
}

async function CreateUser(request: CreateUserRequest): Promise<UserDto> {
    if (!request.email) throw new Error('Email is required');

    if (!request.email.includes('@')) throw new Error('Invalid email');

    const existingUser = await userRepository.findByEmail(request.email);
    if (existingUser) throw new Error('User already exists');

    const user = {
        id: generateUuid(),
        email: request.email,
        firstName: request.firstName,
        lastName: request.lastName,
        createdAt: new Date()
    };

    const savedUser = await userRepository.save(user);

    return {
        id: savedUser.id,
        email: savedUser.email,
        firstName: savedUser.firstName,
        lastName: savedUser.lastName,
        createdAt: savedUser.createdAt.toISOString()
    };
}
```

---

## 4. Краткая версия для устного пересказа

CRUD — Create, Read, Update, Delete. Для User через gRPC делают CreateUser, GetUser, UpdateUser, DeleteUser. Клиент не должен ходить напрямую в PostgreSQL, потому что это небезопасно и обходит бизнес-логику. Правильная цепочка: Web client → backend gRPC API → service → repository → PostgreSQL.

---

## 5. Что обязательно сказать

- CRUD = Create/Read/Update/Delete.
- Клиент не ходит напрямую в БД.
- Backend проверяет данные и права.
- Repository скрывает PostgreSQL.
- Клиент получает DTO.

---

## 6. Типичные ошибки

1. Разрешить Web client подключаться к PostgreSQL.
2. Забыть валидацию email.
3. Сохранить без проверки уникальности.
4. Вернуть Entity вместо DTO.
5. Перепутать service и repository.

---

## 7. Мини-тренировка

1. Что такое CRUD?
2. Какие методы нужны для User?
3. Почему прямой доступ к БД опасен?
4. Что делает Repository?
5. Что возвращает CreateUser?
