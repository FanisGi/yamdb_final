# yamdb_final

Cтатус workflow: ![status](https://github.com/FanisGi/yamdb_final/actions/workflows/workflow.yml/badge.svg)

# Проект YaMDb
## Описание
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.
Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

## Технологии

- Docker
- Docker-compose
- DockerHub
- Python
- Django
- Nginx
- Gunicorn
- PostgreSQL
- CI/CD

## Установка на локальной машине

- Cклонируйте репозитарий 

`git clone git@github.com:FanisGi/api_yamdb.git`

- Cоздайте и активируйте виртуальное окружение:

`python3 -m venv venv`

`python3 venv/bin/activate`

- Установите все зависимости из файла requirements.txt командой: 

`pip install -r requirements.txt`

- Выполните миграции

`python manage.py migrate`

- Запустите веб сервер

`python manage.py runserver`

## Установка на удаленном сервере

- Установите на сервере **docker** и **docker-compose**

- Скопируйте на сервер **docker-compose.yaml** и **nginx/default.conf**

- Создайте копию проекта в своём профиле **github**. 

- В репозитории проекта создайте в **Secrets GitHub** переменные окружения для работы:

```
DB_ENGINE=<django.db.backends.postgresql>
DB_NAME=<имя базы данных postgres>
DB_USER=<пользователь бд>
DB_PASSWORD=<пароль>
DB_HOST=<db>
DB_PORT=<5432>

DOCKER_PASSWORD=<пароль от DockerHub>
DOCKER_USERNAME=<имя пользователя>

SECRET_KEY=<секретный ключ проекта django>

USER=<username для подключения к серверу>
HOST=<IP сервера>
PASSPHRASE=<пароль для сервера, если он установлен>
SSH_KEY=<ваш SSH ключ (для получения команда: cat ~/.ssh/id_rsa)>

TELEGRAM_TO=<ID чата, в который придет сообщение>
TELEGRAM_TOKEN=<токен вашего бота>
```

- При команде `git push` в ветку **master** начнётся выполнения файла **workflows**. Дождитесь завершения всех задач.

## Регистрация 

Алгоритм регистрации пользователей:
1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами email и username на эндпоинт 
`/api/v1/auth/signup/`.
2. **YaMDB** отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
4. При желании пользователь отправляет PATCH-запрос на эндпоинт /api/v1/users/me/ и заполняет поля в своём профайле (описание полей — в документации).

## Пользовательские роли
* **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
* **Аутентифицированный пользователь (user)** — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
* **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
* **Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
* **Суперюзер Django** — обладет правами администратора (admin)

## Примеры API запросов

`GET /categories/`

`Response sample`
`[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]
  }
]`


`POST /categories/`

`Request samples`
`{
  "name": "string",
  "slug": "string"
}`

`Response samples`
`{
  "name": "string",
  "slug": "string"
}`

Полное описание доступно по эндпоинту **/redoc/** - http://84.201.165.117/redoc/

## Автор

Gilazov Fanis
