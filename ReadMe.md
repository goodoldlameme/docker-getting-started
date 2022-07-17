# Задание 1

Собрать и запустить приложение.

## Решение
>docker build -t todo-app .
>docker run -d todo-app

# Задание 2

Приложение слушает 3000 порт.
Запустить приложение так, чтобы оно открывалось на localhost:3000 в браузере.
https://docs.docker.com/config/containers/container-networking/#published

Чтобы узнать, на каком порту приложение слушает, нужно его запустить и посмотреть логи.

Что можно использовать, чтобы знать на какому порту выставлено приложение без запуска?

## Решение
>docker build -t todo-app .
>docker run -d todo-app
>docker logs <container-id> 
Using sqlite database at /etc/todos/todo.db
Listening on port 3000

>docker run -dp 3000:3000 todo-app
-d запускает контейнер в фоновом режиме
-p публикует порты, т.е. связывает 
порты в контейнере с портами на хосте

EXPOSE 3000
Документация между тем, кто пишет Dockerfile и тем, кто запускает контейнер на его основе.

# Задание 3
Поменять в файле app/src/static/js/app.js текст плейсхолдера "New item". 
Перезапустить приложение на том же порту.

## Решение
>docker run -dp 3000:3000 todo-app
...Bind for 0.0.0.0:3000 failed: port is 
already allocated.
>docker ps -a
>docker rm -f <container-id>

# Задание 4
Запушить в docker hub.

## Решение
Залогиниться в Docker Hub.
Repositories > Create Repository.
Назовите репозиторий todo-app.
>docker login
>docker tag todo-app <username>/todo-app
>docker push <username>/todo-app

# Задание 5
В приложении используется база данных SQLite. 
В ней все данные хранятся в одном локальном файле /etc/todos/todo.db

Сделать так, чтобы после перезапуска данные не исчезали.
https://docs.docker.com/storage/volumes/

## Решение
>docker volume create todo-db
>docker run -dp 3000:3000 -v todo-db:/etc/todos todo-app

>docker volume inspect todo-db
[
    {
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
	…
    }
]

# Задание 6
Приложение уже умеет работать c MySQL версии не выше 5.
Сделать так, чтобы приложение писало данные в MySQL.
Приложение и база данных должны работать в одной сети.
В контейнере mysql данные хранятся в /var/lib/mysql.

https://hub.docker.com/_/mysql
https://dev.mysql.com/doc/refman/5.7/en/environment-variables.html
https://docs.docker.com/engine/reference/run/#env-environment-variables

Прочитать про --network-alias.

Переменные окружения необходимые для работы контейнера с базой данных:
MYSQL_ROOT_PASSWORD=secret //пароль
MYSQL_DATABASE=todos //имя базы данных

Переменные окружения необходимые для работы контейнера с приложением:
MYSQL_HOST=mysql // alias
MYSQL_USER=root // пользователь
MYSQL_PASSWORD=secret //пароль
MYSQL_DB=todos // имя базы данных

## Решение
>docker network create todo-app
>docker run -d \
--network todo-app --network-alias mysql \
-v todo-mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=todos \
mysql:5

>docker run -dp 3000:3000 \
--network todo-app \
-e MYSQL_HOST=mysql \
-e MYSQL_USER=root \
-e MYSQL_PASSWORD=secret \
-e MYSQL_DB=todos \
todo-app