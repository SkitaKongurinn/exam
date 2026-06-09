# Билет №12 — PostgreSQL, SQLite, SQL Server, DB, JOIN

## 0. Как понять этот билет простыми словами

Билет про базы данных.

```text
PostgreSQL = основная серверная БД
SQLite = лёгкая локальная БД
SQL Server = корпоративная Microsoft-БД
```

---

## 1. Теоретический вопрос 1

**Вопрос:** Сравните PostgreSQL, SQLite и SQL Server. Когда какую СУБД выбрать?

### Простое объяснение

PostgreSQL подходит для основной CRM. SQLite — для тестов, прототипов и маленьких приложений. SQL Server — для enterprise-среды, особенно в Microsoft-инфраструктуре.

### Готовый ответ на 1 вопрос

PostgreSQL — полноценная серверная СУБД, хорошо подходит для WEB-приложений, CRM и проектов с большим количеством пользователей. Она поддерживает транзакции, связи, индексы и сложные запросы.

SQLite — лёгкая встроенная СУБД, которая хранит данные в одном файле. Её удобно использовать для локальной разработки, тестов и прототипов. SQL Server — корпоративная СУБД Microsoft, часто используется в enterprise-среде. В основной CRM логично выбрать PostgreSQL, для тестов — SQLite, для Microsoft-инфраструктуры — SQL Server.

---

## 2. Теоретический вопрос 2

**Вопрос:** Что означает узел DB на карте знаний? Как унифицировать работу с разными СУБД?

### Простое объяснение

DB — общий слой хранения данных. Унификация делается через Repository.

```text
Service → IRepository → Postgres/SQLite/SQLServer implementation
```

### Готовый ответ на 2 вопрос

Узел DB на карте знаний обозначает слой хранения данных или базу данных в целом. В системе могут использоваться разные СУБД: PostgreSQL, SQLite и SQL Server.

Чтобы бизнес-логика не зависела от конкретной СУБД, используется Repository. Сначала создаётся общий интерфейс репозитория, затем реализации для разных баз. Service работает только с интерфейсом, поэтому конкретную базу можно заменить без изменения бизнес-логики.

---

## 3. Практическое задание

**Задание:** Напишите SQL-запрос: получить пользователей с платежами через JOIN.

### Решение

```sql
SELECT
    u.id AS user_id,
    u.email AS email,
    p.amount AS payment_amount,
    p.payment_date AS payment_date
FROM users u
JOIN payments p ON p.user_id = u.id
ORDER BY p.payment_date DESC;
```

Если нужно вывести пользователей даже без платежей:

```sql
SELECT
    u.id AS user_id,
    u.email AS email,
    p.amount AS payment_amount,
    p.payment_date AS payment_date
FROM users u
LEFT JOIN payments p ON p.user_id = u.id
ORDER BY p.payment_date DESC;
```

---

## 4. Краткая версия для устного пересказа

PostgreSQL — основная серверная БД для WEB/CRM. SQLite — лёгкая файловая БД для тестов и прототипов. SQL Server — enterprise-БД Microsoft. DB на карте — слой хранения данных. Repository унифицирует доступ к разным СУБД.

---

## 5. Что обязательно сказать

- PostgreSQL — основной выбор для WEB/CRM.
- SQLite — локально и для тестов.
- SQL Server — enterprise Microsoft.
- DB = слой хранения.
- Repository унифицирует доступ.

---

## 6. Типичные ошибки

1. Сказать, что SQLite подходит для большой CRM под высокой нагрузкой.
2. Не объяснить DB.
3. Забыть условие JOIN.
4. Перепутать JOIN и LEFT JOIN.
5. Смешать SQL с frontend.

---

## 7. Мини-тренировка

1. Когда выбрать PostgreSQL?
2. Когда выбрать SQLite?
3. Где используется SQL Server?
4. Что означает DB?
5. Как написать JOIN users/payments?
