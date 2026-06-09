# Билет №5 — Component, Input/Output, SubscriptionForm

## 0. Как понять этот билет простыми словами

Компоненты — это «кубики» интерфейса.

```text
SubscriptionForm = форма подписки
SubscriptionCard = карточка подписки
SubscriptionList = список подписок
```

---

## 1. Теоретический вопрос 1

**Вопрос:** Что такое Component в компонентной архитектуре frontend-приложения? Какие преимущества даёт разбиение UI на компоненты?

### Простое объяснение

Component — самостоятельная часть интерфейса: разметка, стили, логика, входные данные и события. Плюсы: код проще читать, компонент можно переиспользовать, легче тестировать, проще менять интерфейс.

### Готовый ответ на 1 вопрос

Component — независимая часть пользовательского интерфейса, которая объединяет разметку, стили и логику. Frontend-приложение строится как набор компонентов.

Разбиение UI на компоненты делает код понятнее, переиспользуемее и удобнее для тестирования. Форму подписки, карточку подписки и список подписок можно реализовать отдельными компонентами, чтобы менять одну часть интерфейса, не ломая остальные.

---

## 2. Теоретический вопрос 2

**Вопрос:** Объясните паттерн Input/Output при передаче данных между родительским и дочерним компонентами. Приведите пример SubscriptionCard.

### Простое объяснение

Input — данные от родителя к дочернему компоненту. Output — событие от дочернего компонента к родителю.

```text
Parent → Input subscription → SubscriptionCard
SubscriptionCard → Output cancel(id) → Parent
```

### Готовый ответ на 2 вопрос

Паттерн Input/Output используется для связи между родительским и дочерним компонентами. Input позволяет родителю передать данные дочернему компоненту. Output позволяет дочернему компоненту сообщить родителю о событии.

Например, `SubscriptionCard` получает через Input объект подписки. Если пользователь нажимает «Отменить», компонент генерирует Output-событие `cancel` с id подписки, а родитель вызывает backend API и обновляет список.

---

## 3. Практическое задание

**Задание:** Напишите TypeScript-компонент `SubscriptionForm` с `@Input() userId` и `@Output() onSubmit`.

### Решение

```ts
import { Component, EventEmitter, Input, Output } from '@angular/core';

type SubscriberType = 1 | 2 | 3;

interface SubscriptionFormData {
    userId: string;
    email: string;
    subscriberType: SubscriberType;
}

@Component({
    selector: 'app-subscription-form',
    template: `
        <form (ngSubmit)="submit()">
            <label>Email</label>
            <input type="email" [(ngModel)]="email" name="email" required />

            <label>Тип подписчика</label>
            <select [(ngModel)]="subscriberType" name="subscriberType" required>
                <option [ngValue]="1">Подписчик 1</option>
                <option [ngValue]="2">Подписчик 2</option>
                <option [ngValue]="3">Подписчик 3</option>
            </select>

            <button type="submit">Подписаться</button>
        </form>
    `
})
export class SubscriptionFormComponent {
    @Input() userId!: string;
    @Output() onSubmit = new EventEmitter<SubscriptionFormData>();

    email = '';
    subscriberType: SubscriberType = 1;

    submit(): void {
        if (!this.userId || !this.email) return;

        this.onSubmit.emit({
            userId: this.userId,
            email: this.email,
            subscriberType: this.subscriberType
        });
    }
}
```

---

## 4. Краткая версия для устного пересказа

Component — самостоятельная часть интерфейса. Input передаёт данные от родителя к дочернему компоненту, Output передаёт событие обратно родителю. SubscriptionForm получает userId через Input, а при отправке формы эмитит данные через Output.

---

## 5. Что обязательно сказать

- Component = часть интерфейса.
- Input = данные внутрь компонента.
- Output = событие наружу.
- Родитель хранит состояние.
- Дочерний компонент отображает и эмитит события.

---

## 6. Типичные ошибки

1. Перепутать Input и Output.
2. Делать дочерний компонент слишком зависимым от backend.
3. Не передать userId.
4. Не эмитить событие родителю.
5. Сказать, что компонент — только HTML.

---

## 7. Мини-тренировка

1. Что такое компонент?
2. Зачем нужен Input?
3. Зачем нужен Output?
4. Что делает SubscriptionCard?
5. Что родитель делает при cancel?
