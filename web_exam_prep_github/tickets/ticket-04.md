# Билет №4 — async, Event Loop, Promise, async/await

## 0. Как понять этот билет простыми словами

Билет про то, как frontend выполняет сетевые запросы и не зависает.

```text
Клиент отправил запрос → браузер не завис → ответ пришёл позже → данные показали пользователю
```

---

## 1. Теоретический вопрос 1

**Вопрос:** Что такое асинхронное программирование в JavaScript/TypeScript? Зачем оно необходимо в Web client при обращении к gRPC API?

### Простое объяснение

Асинхронность позволяет выполнять долгие операции без блокировки интерфейса: запросы к backend, загрузка данных, ожидание сети. Если запрос делать с блокировкой, страница зависнет.

### Готовый ответ на 1 вопрос

Асинхронное программирование в JavaScript/TypeScript — это механизм выполнения операций, результат которых появится позже, без блокировки основного потока интерфейса. Оно необходимо в Web client, потому что обращения к backend gRPC API выполняются по сети и могут занимать время.

Асинхронный код позволяет отправить запрос, дождаться ответа и обработать результат, при этом страница остаётся отзывчивой.

---

## 2. Теоретический вопрос 2

**Вопрос:** Объясните работу Event Loop и механизм Promise. Чем async/await отличается от цепочки .then()/.catch()?

### Простое объяснение

Event Loop управляет выполнением обычного кода и асинхронных задач. Promise — объект результата в будущем.

```text
pending → fulfilled/rejected
```

`then/catch` — callback-цепочки. `async/await` — более читаемый синтаксис поверх Promise с `try/catch`.

### Готовый ответ на 2 вопрос

Event Loop — механизм JavaScript, который позволяет выполнять асинхронные операции без блокировки основного потока. Он следит за стеком вызовов, очередью задач и микрозадачами.

Promise представляет результат асинхронной операции в будущем. Он может быть в состоянии ожидания, успешного выполнения или ошибки. `.then()`/`.catch()` обрабатывают результат через callback-функции. `async/await` работает поверх Promise, но позволяет писать код более последовательно и использовать `try/catch`.

---

## 3. Практическое задание

**Задание:** Напишите TypeScript-функцию `fetchSubscriptions(userId: string): Promise<Subscription[]>`.

### Решение

```ts
interface Subscription {
    id: string;
    userId: string;
    subscriberType: 1 | 2 | 3;
    status: 'active' | 'pending' | 'cancelled';
    createdAt: string;
}

async function fetchSubscriptions(userId: string): Promise<Subscription[]> {
    try {
        if (!userId) throw new Error('userId is required');

        const response = await fetch(`/api/users/${userId}/subscriptions`);

        if (!response.ok) {
            throw new Error(`Network error: ${response.status}`);
        }

        const data = await response.json();
        return data as Subscription[];
    } catch (error) {
        console.error('Failed to fetch subscriptions:', error);
        throw new Error('Не удалось загрузить подписки');
    }
}
```

Вариант через gRPC wrapper:

```ts
async function fetchSubscriptions(userId: string): Promise<Subscription[]> {
    try {
        const response = await subscriptionGrpcClient.getSubscriptions({ userId });
        return response.subscriptions;
    } catch (error) {
        console.error('gRPC request failed:', error);
        throw new Error('Не удалось загрузить подписки');
    }
}
```

---

## 4. Краткая версия для устного пересказа

Async нужен, чтобы сетевые запросы не замораживали интерфейс. Event Loop управляет выполнением синхронного кода и асинхронных задач. Promise представляет результат в будущем. async/await работает поверх Promise и делает код похожим на обычный последовательный код.

---

## 5. Что обязательно сказать

- async нужен для сетевых запросов.
- Promise = результат в будущем.
- Event Loop не даёт интерфейсу зависнуть.
- async/await читабельнее then/catch.
- Ошибки обрабатываются через try/catch.

---

## 6. Типичные ошибки

1. Забыть await.
2. Не обработать ошибку сети.
3. Не вернуть Promise<Subscription[]>.
4. Перепутать then/catch и try/catch.
5. Сказать, что async делает JS многопоточным.

---

## 7. Мини-тренировка

1. Что такое Promise?
2. Какие состояния есть у Promise?
3. Зачем нужен Event Loop?
4. Чем async/await удобнее then?
5. Почему запрос к backend должен быть async?
