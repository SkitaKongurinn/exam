# Билет №10 — DTO, Entity, User и Person

## 0. Как понять этот билет простыми словами

Внутри приложения может быть одна модель, а наружу клиенту отдаётся другая.

```text
Entity = внутренняя сущность
DTO = объект передачи данных
```

Например, в Entity может быть `passwordHash`, а в DTO его быть не должно.

---

## 1. Теоретический вопрос 1

**Вопрос:** Что такое DTO? Зачем отделять DTO от доменных сущностей Entity?

### Простое объяснение

DTO — Data Transfer Object, объект передачи данных. Он нужен, чтобы передавать только нужные и безопасные поля.

```text
User Entity: id, email, passwordHash, role
User DTO: id, email, role
```

### Готовый ответ на 1 вопрос

DTO, или Data Transfer Object, — это объект, который используется для передачи данных между слоями приложения или между backend и клиентом. DTO содержит только те поля, которые нужны получателю.

DTO отделяют от Entity, потому что Entity отражает внутреннюю модель приложения и может содержать служебные или чувствительные данные, например парольный хэш. DTO позволяет скрыть лишние поля, изменить формат ответа для клиента и не привязывать внешний API к внутренней структуре базы данных.

---

## 2. Теоретический вопрос 2

**Вопрос:** В чём различие сущностей User и Person в доменной модели? Когда используется каждая из них?

### Простое объяснение

User — учётная запись: email, login, passwordHash, role, status.

Person — данные человека: имя, фамилия, дата рождения, адрес, телефон.

User нужен для входа и прав, Person — для профиля человека.

### Готовый ответ на 2 вопрос

User и Person отличаются назначением. User — это пользовательская учётная запись, которая используется для входа в систему, аутентификации, авторизации и ролей. В User обычно хранятся email, логин, парольный хэш, роль и статус аккаунта.

Person описывает человека как физическое лицо или профильные данные: имя, фамилию, дату рождения, адрес и телефон. User используется для доступа к системе, а Person — для хранения персональной информации. Эти сущности могут быть связаны, но их не нужно смешивать.

---

## 3. Практическое задание

**Задание:** Опишите TypeScript-интерфейсы `UserDto`, `PersonDto` и функцию `mapEntityToDto`.

### Решение

```ts
interface PersonDto {
    id: string;
    firstName: string;
    lastName: string;
    birthDate?: string;
    phone?: string;
}

interface UserDto {
    id: string;
    email: string;
    role: string;
    status: string;
    person?: PersonDto;
    createdAt: string;
}

interface User {
    id: string;
    email: string;
    passwordHash: string;
    role: string;
    status: string;
    person?: {
        id: string;
        firstName: string;
        lastName: string;
        birthDate?: Date;
        phone?: string;
    };
    createdAt: Date;
}

function mapEntityToDto(user: User): UserDto {
    return {
        id: user.id,
        email: user.email,
        role: user.role,
        status: user.status,
        createdAt: user.createdAt.toISOString(),
        person: user.person ? {
            id: user.person.id,
            firstName: user.person.firstName,
            lastName: user.person.lastName,
            birthDate: user.person.birthDate?.toISOString(),
            phone: user.person.phone
        } : undefined
    };
}
```

Важно: `passwordHash` не попадает в DTO.

---

## 4. Краткая версия для устного пересказа

DTO — объект передачи данных, Entity — внутренняя доменная сущность. DTO нужен, чтобы не отдавать лишние и чувствительные поля. User — аккаунт для входа и прав доступа, Person — данные человека, например имя и адрес.

---

## 5. Что обязательно сказать

- DTO = объект передачи данных.
- Entity = внутренняя модель.
- DTO не содержит passwordHash.
- User = аккаунт и доступ.
- Person = данные человека.

---

## 6. Типичные ошибки

1. Отправить passwordHash в DTO.
2. Смешать User и Person без объяснения.
3. Считать DTO главной таблицей БД.
4. Не объяснить маппинг.
5. Вернуть клиенту всю Entity.

---

## 7. Мини-тренировка

1. Что такое DTO?
2. Чем DTO отличается от Entity?
3. Почему нельзя отдавать passwordHash?
4. Чем User отличается от Person?
5. Что делает mapEntityToDto?
