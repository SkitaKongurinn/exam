# Билет №3. Web client, HTML, CSS

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Опишите структуру Web client в многослойном WEB-приложении. Какие технологии и слои входят в клиентскую часть согласно карте знаний?

### Теоретический вопрос 2

В чём разница зон ответственности HTML и CSS? Приведите пример: что относится к разметке, а что — к оформлению формы регистрации подписчика.

### Практический вопрос

Напишите HTML-разметку формы подписки (поля: email, тип подписчика — select, кнопка «Подписаться») и CSS-стили для центрирования формы и оформления кнопки.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**Web client** — это клиентская часть многослойного WEB-приложения, с которой взаимодействует пользователь через браузер.

По roadmap клиентская часть включает:

- **HTML** — структура страницы;
- **CSS** — оформление и расположение элементов;
- **JavaScript/TypeScript** — логика интерфейса;
- **компоненты** — независимые части UI;
- **Input/Output** — передача данных между компонентами;
- **gRPC wrapper** — клиентская обёртка для обращения к backend API;
- обработку состояний: загрузка, ошибка, успешный ответ.

Типичная структура Web client:

```text
Web client
├── pages
│   └── SubscriptionPage
├── components
│   ├── SubscriptionForm
│   └── SubscriptionCard
├── services
│   └── UserGrpcClient / SubscriptionGrpcClient
├── models
│   └── DTO / interfaces
└── styles
    └── CSS files
```

#### Как лучше ответить преподавателю

Web client — это клиентский слой приложения: HTML задаёт структуру страницы, CSS отвечает за внешний вид, TypeScript/JavaScript реализует поведение, компоненты делят интерфейс на части, а обёртка над gRPC изолирует работу с backend API.

### Теоретический вопрос 2

#### Пояснение

**HTML** отвечает за смысловую структуру страницы. Он описывает, какие элементы есть на странице: форма, поле email, select, кнопка.

**CSS** отвечает за внешний вид: цвет, размер, отступы, центрирование, шрифты, состояние кнопки при наведении.

Пример:

- HTML: «здесь находится форма регистрации подписчика»;
- CSS: «форма должна быть по центру, с белым фоном, тенью и синей кнопкой».

Разделение важно, потому что структура и оформление должны быть независимыми.

#### Как лучше ответить преподавателю

HTML отвечает за смысловую разметку: форма, input, label, select, button. CSS отвечает за оформление: размеры, цвета, отступы, центрирование, состояния кнопки. Например, поле email — это HTML, а его ширина, рамка и цвет кнопки — CSS.

---

## 3. Практика

### Что важно показать

Нужно разделить HTML и CSS: HTML создаёт форму и поля, CSS центрирует блок и оформляет кнопку.

### Готовое решение

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8" />
    <title>Форма подписки</title>
    <link rel="stylesheet" href="style.css" />
</head>
<body>
    <main class="page">
        <form class="subscription-form">
            <h1>Оформить подписку</h1>

            <label for="email">Email</label>
            <input
                id="email"
                name="email"
                type="email"
                placeholder="user@example.com"
                required
            />

            <label for="subscriberType">Тип подписчика</label>
            <select id="subscriberType" name="subscriberType" required>
                <option value="1">Подписчик 1</option>
                <option value="2">Подписчик 2</option>
                <option value="3">Подписчик 3</option>
            </select>

            <button type="submit">Подписаться</button>
        </form>
    </main>
</body>
</html>
```

```css
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f3f5f7;
}

.page {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.subscription-form {
    width: 360px;
    padding: 24px;
    background: #ffffff;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

.subscription-form h1 {
    margin-top: 0;
    font-size: 24px;
}

.subscription-form label {
    display: block;
    margin-top: 16px;
    margin-bottom: 6px;
}

.subscription-form input,
.subscription-form select {
    width: 100%;
    padding: 10px;
    border: 1px solid #c8c8c8;
    border-radius: 8px;
}

.subscription-form button {
    width: 100%;
    margin-top: 20px;
    padding: 12px;
    border: none;
    border-radius: 8px;
    background: #2563eb;
    color: white;
    font-weight: bold;
    cursor: pointer;
}

.subscription-form button:hover {
    background: #1d4ed8;
}
```

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
