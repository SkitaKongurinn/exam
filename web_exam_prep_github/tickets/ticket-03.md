# Билет №3 — Web client, HTML, CSS, форма подписки

## 0. Как понять этот билет простыми словами

Билет про frontend — часть приложения в браузере.

```text
HTML = структура
CSS = оформление
TypeScript/JavaScript = логика
```

---

## 1. Теоретический вопрос 1

**Вопрос:** Опишите структуру Web client в многослойном WEB-приложении. Какие технологии и слои входят в клиентскую часть согласно карте знаний?

### Простое объяснение

Web client состоит из HTML, CSS, TypeScript/JavaScript, компонентов и обёртки над gRPC.

```text
Пользователь → UI-компоненты → TS-логика → gRPC wrapper → backend gRPC API
```

### Готовый ответ на 1 вопрос

Web client — это клиентская часть WEB-приложения, которая работает в браузере и обеспечивает взаимодействие пользователя с системой. Согласно карте знаний, клиентская часть строится на HTML, CSS и TypeScript/JavaScript, а интерфейс организуется через компонентную модель.

HTML отвечает за структуру страницы, CSS — за оформление, TypeScript/JavaScript — за логику интерфейса, обработку событий и обращение к backend. Для взаимодействия с сервером используется обёртка над gRPC, которая скрывает детали протокола.

---

## 2. Теоретический вопрос 2

**Вопрос:** В чём разница зон ответственности HTML и CSS? Приведите пример формы регистрации подписчика.

### Простое объяснение

HTML отвечает за элементы: `form`, `input`, `select`, `button`. CSS отвечает за внешний вид: центрирование, отступы, цвета, рамки, размеры.

```text
HTML = что находится на странице.
CSS = как это выглядит.
```

### Готовый ответ на 2 вопрос

HTML и CSS отвечают за разные части клиентского интерфейса. HTML используется для разметки, то есть для описания структуры страницы и элементов формы. Например, в форме подписки к HTML относятся теги `form`, `input`, `select`, `option` и `button`.

CSS отвечает за оформление этих элементов: расположение формы на странице, размеры полей, отступы, цвет кнопки, шрифт и рамки. HTML отвечает на вопрос «что находится на странице», а CSS — «как это выглядит».

---

## 3. Практическое задание

**Задание:** Напишите HTML-разметку формы подписки и CSS-стили.

### Решение

```html
<form class="subscription-form">
    <h2>Оформить подписку</h2>

    <label for="email">Email</label>
    <input id="email" name="email" type="email" required />

    <label for="subscriberType">Тип подписчика</label>
    <select id="subscriberType" name="subscriberType" required>
        <option value="">Выберите тип</option>
        <option value="1">Подписчик 1</option>
        <option value="2">Подписчик 2</option>
        <option value="3">Подписчик 3</option>
    </select>

    <button type="submit">Подписаться</button>
</form>
```

```css
body {
    margin: 0;
    min-height: 100vh;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
}

.subscription-form {
    width: 320px;
    padding: 24px;
    border: 1px solid #ddd;
    border-radius: 12px;
}

.subscription-form label {
    display: block;
    margin-top: 12px;
    margin-bottom: 6px;
}

.subscription-form input,
.subscription-form select {
    width: 100%;
    padding: 10px;
    box-sizing: border-box;
}

.subscription-form button {
    width: 100%;
    margin-top: 18px;
    padding: 12px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}
```

---

## 4. Краткая версия для устного пересказа

Web client — клиентская часть в браузере. Она состоит из HTML, CSS, TypeScript/JavaScript, компонентов и gRPC wrapper. HTML задаёт структуру страницы, CSS — внешний вид. В форме подписки HTML создаёт email, select и кнопку, а CSS центрирует форму и оформляет элементы.

---

## 5. Что обязательно сказать

- HTML = структура.
- CSS = оформление.
- TS/JS = логика.
- Component = часть интерфейса.
- gRPC wrapper связывает frontend с backend.

---

## 6. Типичные ошибки

1. Сказать, что HTML отвечает за цвет кнопки.
2. Сказать, что CSS создаёт поля формы.
3. Забыть label.
4. Не указать type=email.
5. Не использовать required.

---

## 7. Мини-тренировка

1. За что отвечает HTML?
2. За что отвечает CSS?
3. Что делает TypeScript?
4. Что такое Web client?
5. Зачем нужен gRPC wrapper?
