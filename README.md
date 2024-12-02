# Библиотека книг - Управление библиотекой через API

Консольное приложение для управления библиотекой книг с использованием FastAPI и PostgreSQL. Приложение позволяет добавлять, удалять, искать и отображать книги в библиотеке. Каждая книга содержит уникальный идентификатор, название, автора, год издания и статус (в наличии или выдана).

## Описание функционала

### 1. **Добавление книги**
   Пользователь вводит название книги (`title`), автора (`author`), год издания (`year`), и книга добавляется в библиотеку с уникальным идентификатором и статусом `"в наличии"`.

   **Метод**: `POST /books/add`
   **Тело запроса**:
   ```json
   {
     "title": "Название книги",
     "author": "Автор книги",
     "year": 2023,
     "status": "в наличии"
   }
   ```


### 2. **Удаление книги**
   Пользователь вводит уникальный идентификатор книги (`id`), и книга удаляется из библиотеки. Если книги с таким `id` не существует, возвращается ошибка.

   **Метод**: `DELETE /books/delete`
   **Параметры запроса**:
   ```json
   {
     "book_id": 1
   }
   ```


### 3. **Поиск книги**
   Пользователь может искать книгу по названию (`title`), автору (`author`) или году издания (`year`).

   **Метод**: `GET /books/search`
   **Параметры запроса**:
   - `criterion`: критерий поиска, один из следующих: `"title"`, `"author"`, `"year"`
   - `value`: значение для поиска (например, `"название книги"`, `"автор книги"`, `2023`)

   Пример запроса:
   ```bash
   GET /books/search?criterion=title&value=Название книги
   ```

### 4. **Отображение всех книг**
   Приложение выводит список всех книг в библиотеке с их идентификаторами, названиями, авторами, годами издания и статусами.

   **Метод**: `GET /books/get_all`

### 5. **Изменение статуса книги**
   Пользователь может изменить статус книги на `"в наличии"` или `"выдана"`.

   **Метод**: `PATCH /books/update_status`
   **Параметры запроса**:
   ```json
   {
     "book_id": 1,
     "new_status": "выдана"
   }
   ```

---

## Структура проекта

```
app/
│
├── config.py               # Конфигурация приложения
├── database.py             # Конфигурация подключения к базе данных (SQLAlchemy)
├── database_depends.py     # Модуль для работы с зависимостями базы данных
├── main.py                 # Основной файл с запуском FastAPI приложения
├── models.py               # Модели SQLAlchemy
├── router.py               # Роутеры (API для работы с книгами)
├── schemas.py              # Схемы Pydantic для валидации данных
└── __init__.py             # Инициализация пакета
alembic.ini                 # Конфигурация Alembic для миграций базы данных
```

### Описание файлов:

1. **`database.py`**: Содержит конфигурацию для подключения к базе данных PostgreSQL с использованием SQLAlchemy.
2. **`database_depends.py`**: Осуществляет зависимость для получения сессии базы данных в запросах FastAPI.
3. **`main.py`**: Основной файл приложения, где создаётся экземпляр FastAPI и подключаются роутеры.
4. **`models.py`**: Определение моделей для работы с таблицами базы данных, в частности, модель `Book` для хранения информации о книгах.
5. **`router.py`**: API для работы с книгами (добавление, удаление, поиск, изменение статуса).
6. **`schemas.py`**: Схемы Pydantic для валидации данных, которые отправляются через API.

---

## Установка и запуск

### Требования:
- Python 3.8+
- PostgreSQL

### Шаги для установки:

1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/Raynece/book-library-api.git
   cd book-library-api
   ```

2. Создайте виртуальное окружение и активируйте его:
   ```bash
   python -m venv venv
   source venv/bin/activate  # Для Linux/Mac
   venv\Scripts\activate     # Для Windows
   ```

3. Установите зависимости:
   ```bash
   pip install -r requirements.txt
   ```

4. Запустите базу данных PostgreSQL и создайте базу данных, если это не было сделано ранее.

5. Примените миграции базы данных с помощью Alembic:
   ```bash
   alembic upgrade head
   ```

6. Запустите приложение:
   ```bash
   uvicorn app.main:app --reload
   ```

7. Перейдите в браузер и откройте Swagger UI по адресу:
   ```
   http://127.0.0.1:8000/docs
   ```
   Swagger UI позволяет тестировать все доступные эндпоинты API.

---

