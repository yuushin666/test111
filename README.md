# api_yatube

API-бэкенд для платформы Yatube, реализованный на Django REST Framework.

**Основная задача:** предоставить доступ к моделям постов, групп и комментариев через REST API с авторизацией по токену.

**Автор:** Anton Idk

## Версии API

В проекте реализованы следующие версии API:

### v1 (текущая)
* **Базовый URL:** `/api/v1/`
* **Документация:**
  * Swagger UI: `/api/v1/swagger/`
  * ReDoc: `/api/v1/redoc/`
* **Аутентификация:** токен (TokenAuthentication)

### Как запустить проект:

1. Клонировать репозиторий и перейти в него в командной строке:
```
git clone https://github.com/yuushin666/api-yatube.git
cd api-yatube/
```
2. Cоздать и активировать виртуальное окружение:
- Linux и macOS:
```
python3 -m venv venv
source venv/bin/activate
```
- Windows:
```
python -m venv venv
source venv/Scripts/activate
```
3. Установить зависимости из файла `requirements.txt`:
```
python -m pip install --upgrade pip
pip install -r requirements.txt
```
4. Перейти в основную директорию проекта:
```
cd yatube_api/
```
5. Выполнить миграции:
```
python manage.py migrate
```
6. Запустить проект:
```
python manage.py runserver
```

## Доступные эндпоинты

### Аутентификация
- `POST /api/v1/api-token-auth/` — получение токена (передаём логин и пароль).

### Работа с постами
- `GET /api/v1/posts/` — список всех постов.
- `POST /api/v1/posts/` — создание нового поста.
- `GET /api/v1/posts/{post_id}/` — детальная информация о посте.
- `PUT /api/v1/posts/{post_id}/` — полное обновление поста.
- `PATCH /api/v1/posts/{post_id}/` — частичное обновление поста.
- `DELETE /api/v1/posts/{post_id}/` — удаление поста.

### Группы
- `GET /api/v1/groups/` — список всех групп.
- `GET /api/v1/groups/{group_id}/` — информация о группе.

### Комментарии
- `GET /api/v1/posts/{post_id}/comments/` — список комментариев к посту.
- `POST /api/v1/posts/{post_id}/comments/` — создание комментария.
- `GET /api/v1/posts/{post_id}/comments/{comment_id}/` — детальная информация о комментарии.
- `PUT /api/v1/posts/{post_id}/comments/{comment_id}/` — обновление комментария.
- `PATCH /api/v1/posts/{post_id}/comments/{comment_id}/` — частичное обновление комментария.
- `DELETE /api/v1/posts/{post_id}/comments/{comment_id}/` — удаление комментария.

## Особенности реализации
- **Аутентификация:** `TokenAuthentication` (доступ только для авторизованных пользователей).
- **Разрешения:** `IsAuthenticated` + `IsAuthorOrReadOnly` (редактирование только своего контента).
- **Использование:** `ModelViewSet` для модели `Post` и других ресурсов.
- **Документация:** доступна в `ReDoc` и `Swagger` после запуска сервера.

## Примеры запросов

### Получение токена:
**POST `/api/v1/api-token-auth/`**
```json
{
    "username": "anton",
    "password": "password123"
}
```
**Пример ответа:**
```json
{
  "token": "9944b09199c62bc79dbe68028cb4888625775dad"
}
```

### Добавление нового поста:
**POST `/api/v1/posts/`**
**Тело запроса:**
```json
{
    "text": "Вечером собрались в редакции «Русской мысли», чтобы поговорить о народном театре. Проект Шехтеля всем нравится.",
    "group": 1
}
```
**Пример ответа:**
```json
{
    "id": 14,
    "text": "Вечером собрались в редакции «Русской мысли», чтобы поговорить о народном театре. Проект Шехтеля всем нравится.",
    "author": "anton",
    "image": null,
    "group": 1,
    "pub_date": "2021-06-01T08:47:11.084589Z"
}
```

### Добавление комментария к посту (id=14):
**POST `/api/v1/posts/14/comments/`**
```json
{
    "text": "тест тест"
} 
```
**Пример ответа:**
```json
{
    "id": 4,
    "author": "anton",
    "post": 14,
    "text": "тест тест",
    "created": "2021-06-01T10:14:51.388932Z"
} 
```

### Получение информации о группе (id=2):
**GET `/api/v1/groups/2/`**
**Пример ответа:**
```json
{
    "id": 2,
    "title": "Математика",
    "slug": "math",
    "description": "Посты на тему математики"
}
```
