# Билет №11. Repository, интерфейс, source

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Объясните паттерн Repository. Какую роль играет интерфейс репозитория (I) и чем source отличается от реализации?

### Теоретический вопрос 2

Как Repository обеспечивает абстракцию над разными источниками данных (Postgres, SQLite, SQL Server)?

### Практический вопрос

Напишите интерфейс IUserRepository с методами findById, save, delete и класс PostgresUserRepository, реализующий этот интерфейс (сигнатуры методов, без полной реализации SQL).

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**Repository** — паттерн проектирования, который отделяет бизнес-логику от конкретного способа хранения данных.

Сервис не должен знать, как именно выполняется SQL-запрос. Он работает с интерфейсом:

```text
UserService -> IUserRepository -> PostgresUserRepository -> PostgreSQL
```

**Интерфейс репозитория** задаёт контракт: какие методы доступны. Например:

- `findById(id)`;
- `save(user)`;
- `delete(id)`.

**Source** — источник данных: PostgreSQL, SQLite, SQL Server, внешний API, файл. Реализация репозитория зависит от source, но сервисный слой об этом не знает.

#### Как лучше ответить преподавателю

Repository — это паттерн, который скрывает работу с БД за интерфейсом. Интерфейс `IUserRepository` задаёт контракт методов, например findById/save/delete. Source — это источник данных, например PostgreSQL или SQLite, а реализация — конкретный класс, который умеет обращаться к этому source.

### Теоретический вопрос 2

#### Пояснение

Repository обеспечивает абстракцию так:

```text
IUserRepository
├── PostgresUserRepository
├── SQLiteUserRepository
└── SqlServerUserRepository
```

Сервис вызывает методы интерфейса. Если нужно заменить PostgreSQL на SQLite для тестов, меняется только реализация репозитория, а бизнес-логика остаётся прежней.

Преимущества:

- меньше связность кода;
- проще тестировать;
- можно менять СУБД;
- SQL изолирован в одном слое;
- бизнес-логика становится чище.

#### Как лучше ответить преподавателю

Repository даёт единый интерфейс для разных СУБД. Service работает с `IUserRepository` и не знает, где лежат данные. Можно подставить `PostgresUserRepository`, `SqliteUserRepository` или `SqlServerUserRepository`, не меняя бизнес-логику.

---

## 3. Практика

### Что важно показать

Нужно показать интерфейс и одну конкретную реализацию под PostgreSQL, без обязательной полной SQL-реализации.

### Готовое решение

```ts
interface User {
    id: string;
    email: string;
    firstName: string;
    lastName: string;
}

interface IUserRepository {
    findById(id: string): Promise<User | null>;
    save(user: User): Promise<User>;
    delete(id: string): Promise<void>;
}

class PostgresUserRepository implements IUserRepository {
    constructor(private readonly db: any) {}

    async findById(id: string): Promise<User | null> {
        // SELECT id, email, first_name, last_name FROM users WHERE id = $1
        return null;
    }

    async save(user: User): Promise<User> {
        // INSERT INTO users (...) VALUES (...)
        // или UPDATE users SET ... WHERE id = $1
        return user;
    }

    async delete(id: string): Promise<void> {
        // DELETE FROM users WHERE id = $1
    }
}
```

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
