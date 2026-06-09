# Билет №5. Component, Input/Output

[← Назад к списку билетов](../README.md)

---

## 1. Сам билет

### Теоретический вопрос 1

Что такое Component в компонентной архитектуре frontend-приложения? Какие преимущества даёт разбиение UI на компоненты?

### Теоретический вопрос 2

Объясните паттерн Input/Output (props/events) при передаче данных между родительским и дочерним компонентами. Приведите пример для компонента «SubscriptionCard».

### Практический вопрос

Напишите псевдокод или TypeScript-компонент SubscriptionForm с @Input() userId и @Output() onSubmit, который эмитит данные формы родителю при нажатии кнопки.

---

## 2. Ответы на вопросы

### Теоретический вопрос 1

#### Пояснение

**Component** — это независимая часть пользовательского интерфейса, которая объединяет:

- шаблон HTML;
- стили CSS;
- логику TypeScript/JavaScript;
- входные данные;
- события наружу.

Примеры компонентов:

- `SubscriptionForm` — форма создания подписки;
- `SubscriptionCard` — карточка одной подписки;
- `UserProfile` — профиль пользователя;
- `NotificationList` — список уведомлений.

Преимущества компонентного подхода:

- код проще читать;
- компоненты можно переиспользовать;
- легче тестировать отдельные части;
- проще делить работу между разработчиками;
- UI становится более структурированным.

#### Как лучше ответить преподавателю

Component — это самостоятельный блок интерфейса со своим шаблоном, стилями, состоянием и логикой. Компоненты позволяют переиспользовать UI, легче тестировать код, разделять ответственность и собирать сложный экран из простых частей.

### Теоретический вопрос 2

#### Пояснение

**Input/Output** — паттерн взаимодействия родительского и дочернего компонентов.

- **Input** — данные, которые родитель передаёт дочернему компоненту.
- **Output** — событие, которое дочерний компонент отправляет родителю.

Пример для `SubscriptionCard`:

- родитель передаёт в карточку объект подписки через `Input`;
- карточка показывает статус и тип подписки;
- если пользователь нажал «Отменить», карточка отправляет событие `cancel` через `Output`.

```text
ParentComponent
 ├── передаёт subscription -> SubscriptionCard
 └── слушает событие cancel <- SubscriptionCard
```

#### Как лучше ответить преподавателю

Input передаёт данные от родителя к дочернему компоненту, а Output передаёт событие от дочернего компонента родителю. Например, родитель передаёт в `SubscriptionCard` объект подписки через Input, а карточка отправляет событие `cancel` через Output при нажатии кнопки отмены.

---

## 3. Практика

### Что важно показать

Покажи направление данных: userId приходит в компонент через Input, а данные формы уходят родителю через Output/event emitter.

### Готовое решение

Angular-подобный пример:

```ts
import { Component, EventEmitter, Input, Output } from '@angular/core';

interface SubscriptionFormData {
    userId: string;
    email: string;
    subscriberType: 1 | 2 | 3;
}

@Component({
    selector: 'app-subscription-form',
    template: `
        <form (submit)="submit($event)">
            <input
                type="email"
                placeholder="Email"
                [(ngModel)]="email"
                name="email"
                required
            />

            <select [(ngModel)]="subscriberType" name="subscriberType">
                <option [value]="1">Подписчик 1</option>
                <option [value]="2">Подписчик 2</option>
                <option [value]="3">Подписчик 3</option>
            </select>

            <button type="submit">Подписаться</button>
        </form>
    `
})
export class SubscriptionFormComponent {
    @Input() userId!: string;
    @Output() onSubmit = new EventEmitter<SubscriptionFormData>();

    email = '';
    subscriberType: 1 | 2 | 3 = 1;

    submit(event: Event): void {
        event.preventDefault();

        this.onSubmit.emit({
            userId: this.userId,
            email: this.email,
            subscriberType: this.subscriberType
        });
    }
}
```

---

## Мини-шпаргалка перед ответом

- Сначала дай определение ключевого термина из билета.
- Потом свяжи тему с общей архитектурой: **Web client → gRPC → backend → Repository/DB/Redis/Kafka**.
- На практике проговори не только код или схему, но и зачем нужен каждый шаг.
