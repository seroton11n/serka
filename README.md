# CI/CD Pipeline для GitHub Actions
## Цели проекта

**Цель проекта** — настроить простой и понятный CI/CD pipeline, который автоматически:  Запускается при изменениях в репозитории.  Проверяет и тестирует код.  Собирает проект.  Использует секреты GitHub, если они нужны.

Работает без участия человека — push сделал → pipeline всё выполнил сам.
-----------

Этапы работы pipeline  Триггер — что запускает процесс (push или pull request).  Checkout — GitHub скачивает код.  Установка зависимостей — npm/pip/etc.  Тестирование — проверка работоспособности.  Сборка — создание итогового билда.  Развёртывание (опционально).

| № | Этап     | Что делает                           | Когда запускается            |
| - | -------- | ------------------------------------ | ---------------------------- |
| 1 | Триггер  | Запускает pipeline                   | При push/PR в `main`         |
| 2 | Checkout | Скачивает репозиторий                | После триггера               |
| 3 | Install  | Устанавливает зависимости            | После checkout               |
| 4 | Test     | Запускает тесты                      | После установки зависимостей |
| 5 | Build    | Собирает проект                      | Если тесты успешны           |
| 6 | Deploy   | Выполняет развёртывание (если нужно) | После сборки                 |

--------
Файл: .github/workflows/ci-cd.yml

 name: CI/CD Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build_and_test:
    runs-on: ubuntu-latest

    steps:
      - name: Скачать код
        uses: actions/checkout@v3

      - name: Установить зависимости
        run: npm install

      - name: Запустить тесты
        run: npm test

      - name: Собрать проект
        run: npm run build
--------
## Как добавить секреты  

Чтобы использовать токены, ключи или пароли:
1. Перейти в репозиторий.
2. Открыть Settings → Secrets and variables → Actions.
3. Нажать New repository secret.
4. Ввести имя и значение.
Использование секрета в workflow:

```
env:
  MY_SECRET: ${{ secrets.MY_SECRET }}
```
--------
## Как активировать pipeline

Pipeline запускается:
автоматически при:
- push в main
- pull request в main
вручную через:
- Actions
- выбор workflow
- кнопка Run workflow
