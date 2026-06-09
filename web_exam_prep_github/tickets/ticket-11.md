# Билет №11 — Repository, интерфейс, source, разные БД

## 0. Как понять этот билет простыми словами

Repository — слой, который прячет работу с базой.

```text
Service → IUserRepository → PostgresUserRepository → PostgreSQL
```

Service не должен знать SQL и конкретную СУБД.

---

## 1. Теоретический вопрос 1

**Вопрос:** Объясните паттерн Repository. Какую роль играет интерфейс репозитория (I) и чем source отличается от реализации?

### Простое объяснение

Repository предоставляет методы для работы с данными: `findById`, `save`, `delete`.

Интерфейс задаёт контракт. Реализация показывает, как именно работать с конкретным source. Source — источник данных: PostgreSQL, SQLite, SQL Server, API.

### Готовый ответ на 1 вопрос

Паттерн Repository используется для отделения бизнес-логики от конкретного способа хранения данных. Repository предоставляет методы для работы с доменными сущностями, например `findById`, `save`, `delete`, а детали SQL-запросов остаются внутри реализации.

Интерфейс репозитория задаёт контракт, то есть список методов. Бизнес-логика зависит от интерфейса, а не от конкретной базы данных. Source — источник данных, например PostgreSQL или SQLite, а реализация репозитория — конкретный класс, который умеет работать с этим источником.

---

## 2. Теоретический вопрос 2

**Вопрос:** Как Repository обеспечивает абстракцию над разными источниками данных: Postgres, SQLite, SQL Server?

### Простое объяснение

Один интерфейс и несколько реализаций:

```text
IUserRepository
PostgresUserRepository
SQLiteUserRepository
SqlServerUserRepository
```

Service работает с `IUserRepository`, поэтому БД можно заменить без изменения бизнес-логики.

### Готовый ответ на 2 вопрос

Repository обеспечивает абстракцию через интерфейс. Service-слой зависит от `IUserRepository`, а не от `PostgresUserRepository`. Поэтому можно заменить PostgreSQL на SQLite или SQL Server, не меняя бизнес-логику.

Для каждой СУБД создаётся отдельная реализация интерфейса. Все реализации имеют одинаковые методы, но внутри используют разные SQL-диалекты или драйверы. Это делает приложение гибким и упрощает тестирование.

---

## 3. Практическое задание

**Задание:** Напишите интерфейс `IUserRepository` и класс `PostgresUserRepository`.

### Решение

```ts
interface User {
    id: string;
    email: string;
    firstName: string;
    lastName: string;
    createdAt: Date;
}

interface IUserRepository {
    findById(id: string): Promise<User | null>;
    save(user: User): Promise<User>;
    delete(id: string): Promise<void>;
}

class PostgresUserRepository implements IUserRepository {
    constructor(private readonly db: any) {}

    async findById(id: string): Promise<User | null> {
        const result = await this.db.query(
            'SELECT * FROM users WHERE id = $1',
            [id]
        );
        return result.rows[0] ?? null;
    }

    async save(user: User): Promise<User> {
        await this.db.query(
            `INSERT INTO users (id, email, first_name, last_name, created_at)
             VALUES ($1, $2, $3, $4, $5)
             ON CONFLICT (id)
             DO UPDATE SET
                email = EXCLUDED.email,
                first_name = EXCLUDED.first_name,
                last_name = EXCLUDED.last_name`,
            [user.id, user.email, user.firstName, user.lastName, user.createdAt]
        );
        return user;
    }

    async delete(id: string): Promise<void> {
        await this.db.query('DELETE FROM users WHERE id = $1', [id]);
    }
}
```

---

## 4. Краткая версия для устного пересказа

Repository скрывает доступ к данным. Интерфейс задаёт методы, а реализация работает с конкретной БД. Service зависит от интерфейса, поэтому можно заменить PostgreSQL на SQLite или SQL Server без изменения бизнес-логики.

---

## 5. Что обязательно сказать

- Repository скрывает БД.
- Интерфейс задаёт контракт.
- Service зависит от интерфейса.
- Source = источник данных.
- Разные БД = разные реализации.

---

## 6. Типичные ошибки

1. Писать SQL прямо в service.
2. Не использовать интерфейс.
3. Сказать, что Repository — это сама БД.
4. Не отличить интерфейс от реализации.
5. Смешать все БД в одном классе.

---

## 7. Мини-тренировка

1. Что делает Repository?
2. Зачем IUserRepository?
3. Чем интерфейс отличается от реализации?
4. Что такое source?
5. Как заменить PostgreSQL на SQLite?
