# Билет №8 — обёртка над gRPC, backend layers, UserGrpcClient

## 0. Как понять этот билет простыми словами

Обёртка над gRPC — удобный клиентский класс, который скрывает детали вызова.

```ts
userClient.getUser(id)
```

Компонент вызывает простой метод, а внутри создаётся request, вызывается gRPC и возвращается DTO.

---

## 1. Теоретический вопрос 1

**Вопрос:** Зачем нужна обёртка над gRPC на стороне Web client? Какие задачи она решает?

### Простое объяснение

Обёртка нужна, чтобы UI-компоненты не работали напрямую с низкоуровневым gRPC.

Она скрывает протокол, создаёт request, обрабатывает ошибки, добавляет auth token, маппит response в DTO.

### Готовый ответ на 1 вопрос

Обёртка над gRPC на стороне Web client нужна для того, чтобы скрыть технические детали взаимодействия с backend API. Компоненты интерфейса не должны напрямую создавать низкоуровневые gRPC-запросы и разбирать ответы.

Такая обёртка предоставляет удобные типизированные методы, например `getUser(id)` или `createUser(dto)`, внутри формирует request, вызывает gRPC-метод, обрабатывает ошибки, добавляет авторизационные данные и преобразует ответ в DTO.

---

## 2. Теоретический вопрос 2

**Вопрос:** Опишите слои backend grpc API: transport, service, repository. Как организованы вызовы от клиента к серверу?

### Простое объяснение

Слои:

```text
transport gRPC = принимает запросы
service = бизнес-логика
repository = работа с БД
```

Цепочка:

```text
Web client → gRPC wrapper → backend transport → service → repository → PostgreSQL
```

### Готовый ответ на 2 вопрос

Backend gRPC API обычно делится на transport, service и repository. Transport-слой принимает gRPC-запросы и преобразует их во внутренние вызовы. Service-слой содержит бизнес-логику, проверки и сценарии. Repository отвечает за доступ к данным и скрывает детали конкретной базы.

Вызов идёт так: Web client через обёртку вызывает метод backend API. Transport принимает request, передаёт данные в service, service обращается к repository, repository работает с PostgreSQL и возвращает данные обратно по цепочке.

---

## 3. Практическое задание

**Задание:** Напишите TypeScript-класс `UserGrpcClient` с методом `getUser(id: string)`.

### Решение

```ts
interface UserDto {
    id: string;
    email: string;
    firstName: string;
    lastName: string;
}

interface GrpcUserService {
    getUser(request: { id: string }): Promise<{
        id: string;
        email: string;
        first_name: string;
        last_name: string;
    }>;
}

class UserGrpcClient {
    constructor(private readonly grpcService: GrpcUserService) {}

    async getUser(id: string): Promise<UserDto> {
        try {
            if (!id) throw new Error('User id is required');

            const response = await this.grpcService.getUser({ id });

            return {
                id: response.id,
                email: response.email,
                firstName: response.first_name,
                lastName: response.last_name
            };
        } catch (error) {
            console.error('Failed to load user:', error);
            throw new Error('Не удалось получить пользователя');
        }
    }
}
```

---

## 4. Краткая версия для устного пересказа

gRPC wrapper нужен, чтобы компоненты не работали напрямую с raw gRPC. Он скрывает детали протокола, обрабатывает ошибки и возвращает DTO. Backend состоит из transport, service и repository. Вызов идёт от клиента через wrapper в backend, затем в service, repository и БД.

---

## 5. Что обязательно сказать

- gRPC wrapper скрывает детали протокола.
- Компоненты вызывают удобные методы.
- Transport принимает gRPC-запрос.
- Service содержит бизнес-логику.
- Repository работает с БД.

---

## 6. Типичные ошибки

1. Перенести бизнес-логику в компонент.
2. Заставить компонент работать с raw gRPC.
3. Не обработать ошибку.
4. Перепутать service и repository.
5. Вернуть Entity вместо DTO.

---

## 7. Мини-тренировка

1. Зачем gRPC-обёртка?
2. Что делает transport?
3. Что делает service?
4. Что делает repository?
5. Почему клиенту нужен DTO?
