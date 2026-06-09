# Билет №19 — Git workflow, PR/MR, review, merge conflicts, semver

## 0. Как понять этот билет простыми словами

Билет про командную разработку.

```text
feature branch → commits → push → PR/MR → review → merge → tag
```

---

## 1. Теоретический вопрос 1

**Вопрос:** Опишите Git-workflow с feature-ветками. Какова роль lead и frontender?

### Простое объяснение

Не работаем сразу в `main`. Создаём feature-ветку, делаем commits, push, PR/MR, review, исправления, merge.

Frontender делает клиентскую часть. Lead проверяет архитектуру, качество кода и принимает решение о merge.

### Готовый ответ на 1 вопрос

Git-workflow с feature-ветками предполагает, что новая задача выполняется не напрямую в main, а в отдельной ветке. Разработчик обновляет main, создаёт ветку, делает изменения и commits, отправляет ветку в удалённый репозиторий и создаёт Pull Request или Merge Request.

После этого выполняется code review. Lead или другие участники проверяют качество кода, архитектуру, стиль, ошибки и соответствие задаче. Frontender отвечает за компоненты, формы, стили и взаимодействие с API. Lead следит за качеством решения и принимает решение о merge.

---

## 2. Теоретический вопрос 2

**Вопрос:** Что такое merge conflicts и как их разрешать? Объясните semver: alfa, beta, release.

### Простое объяснение

Merge conflict — Git не может автоматически объединить изменения, например две ветки изменили одну строку.

Решение: открыть файл, найти `<<<<<<<`, выбрать/объединить код, проверить, commit.

Semver:

```text
MAJOR.MINOR.PATCH
0.0.0-alpha.1 = ранняя нестабильная
0.0.0-beta.1 = тестовая
1.0.0 = release
```

### Готовый ответ на 2 вопрос

Конфликт слияния возникает, когда Git не может автоматически объединить изменения из разных веток. Обычно это происходит, если два разработчика изменили одни и те же строки файла. Нужно открыть файл, выбрать нужную версию или объединить изменения вручную, проверить код и сделать commit.

Semver, или семантическое версионирование, имеет формат `MAJOR.MINOR.PATCH`. MAJOR меняется при несовместимых изменениях, MINOR — при добавлении функциональности, PATCH — при исправлениях. Alpha — ранняя нестабильная версия, beta — версия для тестирования, release — стабильный выпуск.

---

## 3. Практическое задание

**Задание:** Перечислите Git-команды для ветки `feat_add_subscription`, 2 commit, PR, merge, tag `0.0.0-alfa.1`.

### Решение

```bash
git checkout main
git pull origin main

git checkout -b feat_add_subscription

git add .
git commit -m "Add subscription model"

git add .
git commit -m "Add subscription form"

git push -u origin feat_add_subscription
```

Дальше создаём PR/MR в GitHub/GitLab.

После review:

```bash
git checkout main
git pull origin main

git merge feat_add_subscription
git push origin main

git tag 0.0.0-alfa.1
git push origin 0.0.0-alfa.1
```

Если merge делается кнопкой в GitHub, локально после него:

```bash
git checkout main
git pull origin main
git tag 0.0.0-alfa.1
git push origin 0.0.0-alfa.1
```

---

## 4. Краткая версия для устного пересказа

Git workflow: feature-ветка от main, commits, push, PR/MR, review, merge в main и tag. Frontender реализует клиентскую часть, lead проверяет архитектуру и качество. Merge conflict — конфликт изменений, который решается вручную. Semver — версии MAJOR.MINOR.PATCH; alpha — ранняя, beta — тестовая, release — стабильная.

---

## 5. Что обязательно сказать

- Не работаем напрямую в main.
- Feature-ветка под задачу.
- PR/MR нужен для review.
- Merge conflict решается вручную.
- Semver = MAJOR.MINOR.PATCH.

---

## 6. Типичные ошибки

1. Делать commit сразу в main.
2. Забыть pull перед веткой.
3. Не создать PR/MR.
4. Удалить чужие изменения при конфликте.
5. Перепутать alpha/beta/release.

---

## 7. Мини-тренировка

1. Зачем feature-ветки?
2. Что такое PR/MR?
3. Кто делает review?
4. Что такое merge conflict?
5. Что означает 1.2.3?
