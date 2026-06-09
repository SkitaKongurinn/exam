# Билет №10. DTO, Entity, User и Person

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Что такое DTO (Data Transfer Object)? Зачем отделять DTO от доменных сущностей Entity при передаче данных между слоями?

### Теоретический вопрос 2

В чём различие сущностей User и Person в доменной модели? Когда используется каждая из них?

### Практический вопрос

Опишите TypeScript-интерфейсы UserDto, PersonDto и функцию mapEntityToDto(user: User): UserDto, выполняющую маппинг Entity → DTO.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**DTO (Data Transfer Object)** — объект для передачи данных между слоями или сервисами. DTO содержит только те поля, которые нужно передать наружу или получить снаружи.

**Entity** — доменная сущность, которая отражает внутреннюю бизнес-модель. Она может содержать методы, внутренние поля, связи и правила.

Почему DTO отделяют от Entity:

- чтобы не отдавать лишние внутренние поля клиенту;
- чтобы скрыть структуру БД;
- чтобы не связывать API с доменной моделью;
- чтобы безопасно контролировать формат ответа;
- чтобы легче менять внутреннюю реализацию.

Например, Entity `User` может хранить `passwordHash`, но в `UserDto` этого поля быть не должно.

#### Как лучше ответить преподавателю

DTO — это объект передачи данных между слоями или сервисами. Его отделяют от Entity, потому что Entity отражает внутреннюю доменную модель и может содержать лишние поля, связи и служебную логику. DTO отдаёт наружу только нужные безопасные данные.

### Теоретический вопрос 2

#### Пояснение

`Person` и `User` — похожие, но разные сущности.

**Person** — физическое лицо, данные о человеке:

- имя;
- фамилия;
- дата рождения;
- адрес;
- телефон.

**User** — учётная запись в системе:

- email/login;
- passwordHash;
- role;
- status;
- связь с Person.

Пример: человек Иван Иванов — это `Person`. Его аккаунт для входа в CRM — это `User`. Один `Person` может быть связан с одним или несколькими аккаунтами, в зависимости от модели.

#### Как лучше ответить преподавателю

User — это системная сущность учётной записи: логин, email, роль, статус, id. Person — это данные о человеке: имя, фамилия, адрес, дата рождения. User нужен для входа и прав доступа, Person — для персональной информации в CRM.

---

## 3. Практика

### Что важно показать

Практика проверяет, понимаешь ли ты разницу между внутренней Entity и внешним DTO, поэтому покажи маппинг и не отдавай лишние поля.

### Готовое решение

```ts
interface PersonDto {
    id: string;
    firstName: string;
    lastName: string;
    birthDate?: string;
}

interface UserDto {
    id: string;
    email: string;
    role: 'admin' | 'subscriber';
    status: 'active' | 'blocked';
    person: PersonDto;
}

interface Person {
    id: string;
    firstName: string;
    lastName: string;
    birthDate?: Date;
}

interface User {
    id: string;
    email: string;
    passwordHash: string;
    role: 'admin' | 'subscriber';
    status: 'active' | 'blocked';
    person: Person;
}

function mapEntityToDto(user: User): UserDto {
    return {
        id: user.id,
        email: user.email,
        role: user.role,
        status: user.status,
        person: {
            id: user.person.id,
            firstName: user.person.firstName,
            lastName: user.person.lastName,
            birthDate: user.person.birthDate?.toISOString()
        }
    };
}
```

Важно: `passwordHash` не попадает в DTO.

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
