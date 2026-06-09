# Билет №4. Async, Event Loop, Promise

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Что такое асинхронное программирование (async) в JavaScript/TypeScript? Зачем оно необходимо в Web client при обращении к gRPC API?

### Теоретический вопрос 2

Объясните работу Event Loop и механизм Promise. Чем async/await отличается от цепочки .then()/.catch()?

### Практический вопрос

Напишите TypeScript-функцию fetchSubscriptions(userId: string): Promise<Subscription[]> с использованием async/await, которая выполняет HTTP/gRPC-запрос к backend и обрабатывает ошибку сети.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**Асинхронное программирование** в JavaScript/TypeScript — это способ выполнять длительные операции без блокировки интерфейса. Например, запрос к backend gRPC API может занять время. Если выполнять его синхронно, страница «замрёт». Асинхронность позволяет отправить запрос, продолжить работу интерфейса, а результат обработать позже.

В Web client async нужен для:

- запросов к backend API;
- загрузки списка подписок;
- отправки формы;
- обработки ошибок сети;
- отображения состояния loading/error/success.

#### Как лучше ответить преподавателю

Асинхронность в JS/TS позволяет запускать сетевые запросы без блокировки интерфейса. Когда Web client обращается к gRPC API, ответ может прийти не сразу, поэтому код должен ждать результат через Promise/async-await и при этом не замораживать страницу.

### Теоретический вопрос 2

#### Пояснение

**Event Loop** — механизм JavaScript, который управляет выполнением синхронного кода и асинхронных задач.

Упрощённо:

1. Синхронный код выполняется в Call Stack.
2. Асинхронные операции передаются во внешнюю среду: браузер или Node.js.
3. После завершения результат попадает в очередь задач.
4. Event Loop проверяет, свободен ли Call Stack.
5. Если свободен, callback или продолжение Promise выполняется.

**Promise** — объект, который представляет результат асинхронной операции. У него есть состояния:

- `pending` — ожидание;
- `fulfilled` — успешно выполнено;
- `rejected` — ошибка.

`async/await` — более читаемый синтаксис над Promise. Он позволяет писать асинхронный код почти как обычный последовательный код.

```ts
// Promise chain
api.getUser(id)
  .then(user => console.log(user))
  .catch(error => console.error(error));

// async/await
try {
  const user = await api.getUser(id);
  console.log(user);
} catch (error) {
  console.error(error);
}
```

#### Как лучше ответить преподавателю

Event Loop управляет выполнением: синхронный код идёт в call stack, а результаты асинхронных операций попадают в очереди и выполняются позже. Promise — объект будущего результата. async/await делает работу с Promise похожей на обычный последовательный код, а ошибки удобно ловятся через try/catch; `.then()`/`.catch()` — более цепочный стиль.

---

## 3. Практика

### Что важно показать

Главное — вернуть Promise, использовать async/await, сделать запрос через клиентскую обёртку и обработать сетевую ошибку.

### Готовое решение

```ts
interface Subscription {
    id: string;
    userId: string;
    subscriberType: 1 | 2 | 3;
    status: 'pending' | 'active' | 'cancelled' | 'expired';
    createdAt: string;
}

async function fetchSubscriptions(userId: string): Promise<Subscription[]> {
    try {
        const response = await fetch(`/api/grpc/subscriptions?userId=${userId}`);

        if (!response.ok) {
            throw new Error(`Ошибка backend: ${response.status}`);
        }

        const data = await response.json();
        return data.subscriptions as Subscription[];
    } catch (error) {
        console.error('Не удалось загрузить подписки', error);
        throw new Error('Ошибка сети или backend API');
    }
}
```

Если используется настоящая gRPC-обёртка, вместо `fetch` будет вызов метода клиента, например `subscriptionGrpcClient.getByUserId(userId)`.

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
