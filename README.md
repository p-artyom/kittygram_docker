# Kittygram

## Описание

_Kittygram_ — социальная сеть для обмена фотографиями любимых питомцев. Это
полностью рабочий проект, который состоит из бэкенд-приложения на _Django_ и
фронтенд-приложения на _React_. В данном проекте мной был настроен запуск в
контейнерах и _CI/CD_ с помощью _GitHub Actions_. 

## Технологии

- Python 3.9;
- PostgreSQL 13;
- Django 3.2.3;
- Django REST framework 3.12.4;
- Gunicorn 20.1.0;
- Nginx 1.22.1;
- Node.js 18.

## Деплой проекта на удалённый сервер

Инструкция написана для сервера с установленной _ОС Ubuntu 22.04.1 LTS_.

- Подключитесь к удалённому серверу;

- Установите _Docker Compose_ на сервер. Поочерёдно выполните на сервере
команды для установки _Docker_ и _Docker Compose_ для _Linux_. Выполнять их
лучше в домашней директории пользователя:

```text
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

- Находясь на удалённом сервере, из любой директории выполните команду:

```text
sudo apt install nginx -y
```

- А потом запустите _Nginx_ командой:

```text
sudo systemctl start nginx
```

- На сервере в редакторе _nano_ откройте конфиг _Nginx_:
`sudo nano /etc/nginx/sites-enabled/default`. Измените настройки `location`
в секции `server`:

```text
location / {
    proxy_pass http://127.0.0.1:9000;
    client_max_body_size 20M;
}
```

- Для проверки файла конфигурации на ошибки выполните команду:

```text
sudo nginx -t
```

- Далее перезагрузите конфигурацию _Nginx_:

```text
sudo systemctl reload nginx
```

- Форкните и клонируйте к себе на компьютер
[репозиторий](https://github.com/p-artyom/kittygram_docker);

- В корне проекта создайте директорию `.github/workflows`, а в ней - файл
`main.yml`;

- Скопируйте содержимое файла `kittyfram_workflows` в `main.yml`;

- В строках `73, 118, 141` замените `<your_login>` на свой логин в
_Docker Hub_.

Для автоматического развёртывания контейнеров на удалённом сервере при помощи
_GitHub Actions_ необходимо создать _secrets_ в собственном репозитории.

- Перейдите в настройки репозитория — _Settings_, выберите на панели слева
_Secrets and Variables_ → _Actions_, нажмите _New repository secret_ и
создайте следующие переменные с необходимыми значениями:

  - в переменной `DOCKER_USERNAME` введите имя пользователя в _Docker Hub_;

  - в переменной `DOCKER_PASSWORD` введите пароль пользователя в _Docker Hub_;

  - в переменной `SSH_KEY` должно быть содержимое файла с закрытым _SSH_-ключом
  для доступа к серверу;

  - в переменной `SSH_PASSPHRASE` должно быть значение _passphrase_ для
  доступа к серверу;

  - в переменной `USER` должно быть ваше имя пользователя на сервере;

  - в переменной `HOST` должно быть значение _IP_-адреса вашего сервера;

  - в переменной `POSTGRES_DB` должно быть название базы данных;

  - в переменной `POSTGRES_USER` должно быть имя пользователя БД;

  - в переменной `POSTGRES_PASSWORD` должен быть пароль пользователя БД;

  - в переменной `TELEGRAM_TO` сохраните _ID_ своего телеграм-аккаунта. Узнать
  свой _ID_ можно у телеграм-бота _@userinfobot_. Бот станет отправлять
  уведомления в аккаунт с указанным _ID_;

  - в переменной `TELEGRAM_TOKEN` сохраните токен вашего бота. Получить этот
  токен можно у телеграм-бота _@BotFather_.

- Сделайте коммит и запушьте изменения на _GitHub_;

- Пуш в ветку _main_ запускает тестирование и деплой _Kittygram_ на удалённый
сервер, а после успешного деплоя вам приходит сообщение в Телеграм.

Текущий _Workflow_ состоит из четырёх фаз:

- Тестирование проекта;

- Сборка и публикация образа;

- Автоматический деплой;

- Отправка уведомления в персональный чат.

## Автор

Пилипенко Артем