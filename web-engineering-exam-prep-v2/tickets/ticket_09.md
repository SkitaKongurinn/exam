# Билет №9. CRUD через backend gRPC API

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Как реализовать CRUD-операции через backend grpc API? Опишите методы Create, Read, Update, Delete для сущности User.

### Теоретический вопрос 2

Сравните два подхода: прямая работа Web client с PostgreSQL и работа через gRPC. Почему прямой доступ к БД с клиента недопустим?

### Практический вопрос

Напишите псевдокод gRPC-метода CreateUser: валидация входных данных, сохранение в PostgreSQL через Repository, возврат созданного User DTO.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**CRUD** — базовые операции с сущностью:

- **Create** — создать;
- **Read** — прочитать;
- **Update** — обновить;
- **Delete** — удалить.

Для сущности `User` в backend gRPC API можно сделать методы:

```proto
service UserService {
  rpc CreateUser(CreateUserRequest) returns (UserResponse);
  rpc GetUser(GetUserRequest) returns (UserResponse);
  rpc UpdateUser(UpdateUserRequest) returns (UserResponse);
  rpc DeleteUser(DeleteUserRequest) returns (DeleteUserResponse);
}
```

Логика:

- `CreateUser` валидирует данные и сохраняет пользователя;
- `GetUser` ищет пользователя по id;
- `UpdateUser` проверяет существование и обновляет поля;
- `DeleteUser` удаляет пользователя или помечает как удалённого.

#### Как лучше ответить преподавателю

CRUD через backend gRPC API — это набор методов Create, Read, Update и Delete. Каждый метод принимает DTO/request, проверяет права и валидность данных, вызывает сервис/репозиторий и возвращает результат или ошибку.

### Теоретический вопрос 2

#### Пояснение

Прямой доступ Web client к PostgreSQL недопустим.

Причины:

- нельзя отдавать клиенту логин и пароль от БД;
- пользователь может обойти бизнес-логику;
- невозможно нормально проверить права доступа;
- открывается риск SQL-инъекций и утечки данных;
- клиент становится жёстко связан со схемой БД;
- нельзя централизованно логировать и аудитить операции.

Правильный подход:

```text
Web client -> gRPC API -> Service -> Repository -> PostgreSQL
```

Так backend контролирует валидацию, авторизацию, транзакции, аудит и формат ответа.

#### Как лучше ответить преподавателю

Клиент не должен подключаться к PostgreSQL напрямую, потому что тогда раскрываются доступы к БД, обходится авторизация и бизнес-логика, сложнее контролировать валидацию и аудит. Правильный вариант — клиент вызывает backend через gRPC, а backend уже безопасно работает с БД.

---

## 3. Практика

### Что важно показать

В псевдокоде важно показать валидацию, проверку существования, вызов Repository, сохранение и возврат DTO.

### Готовое решение

Псевдокод `CreateUser`:

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

async function createUser(request: CreateUserRequest): Promise<UserDto> {
    if (!request.email || !request.email.includes('@')) {
        throw new Error('Некорректный email');
    }

    const existingUser = await userRepository.findByEmail(request.email);
    if (existingUser) {
        throw new Error('Пользователь уже существует');
    }

    const user = {
        id: crypto.randomUUID(),
        email: request.email,
        firstName: request.firstName,
        lastName: request.lastName,
        createdAt: new Date()
    };

    const savedUser = await userRepository.save(user);

    await auditService.write({
        action: 'CREATE_USER',
        entityType: 'User',
        entityId: savedUser.id,
        newValue: savedUser
    });

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

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
