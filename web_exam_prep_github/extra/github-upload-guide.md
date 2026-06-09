# Как залить материалы на GitHub

## Вариант 1 — через сайт GitHub

1. Создай новый репозиторий на GitHub.
2. Назови его, например: `web-exam-prep`.
3. Скачай ZIP из ChatGPT.
4. Распакуй архив.
5. На странице репозитория нажми **Add file → Upload files**.
6. Перетащи все файлы и папки из распакованного архива.
7. Нажми **Commit changes**.

## Вариант 2 — через Git

```bash
git init
git add .
git commit -m "Add web exam preparation tickets"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/web-exam-prep.git
git push -u origin main
```

## Как учить с GitHub

Открывай файлы по порядку:

```text
tickets/ticket-01.md
tickets/ticket-02.md
...
tickets/ticket-20.md
```

После каждого билета отвечай вслух на вопросы из блока «Мини-тренировка».
