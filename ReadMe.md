# Задание 1

Приложение слушает 3000 порт.
Собрать и запустить приложение так, чтобы оно открывалось на localhost:3000 в браузере.

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

# Задание 2
Поменять текст плейсхолдера. 
Перезапустить приложение на том же порту.
>docker run -dp 3000:3000 todo-app
...Bind for 0.0.0.0:3000 failed: port is 
already allocated.
>docker ps -a
>docker rm -f <container-id>

# Задание 3
Запушить в docker hub
Залогиниться в Docker Hub.
Repositories > Create Repository.
Назовите репозиторий todo-app.
>docker login
>docker tag todo-app <username>/todo-app
>docker push <username>/todo-app

# Задание 4
В приложении используется база данных SQLite. 
В ней все данные хранятся в одном локальном файле /etc/todos/todo.db

Сделать так, чтобы после перезапуска данные не исчезали.

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

# Задание 5
Приложение уже умеет работать c MySQL
https://hub.docker.com/_/mysql
https://dev.mysql.com/doc/refman/5.7/en/environment-variables.html
Сделать так, чтобы приложение писало данные в MySQL

>docker network create todo-app
>docker run -d \
--network todo-app --network-alias mysql \
-v todo-mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=todos \
mysql

>docker run -dp 3000:3000 \
--network todo-app \
-e MYSQL_HOST=mysql \
-e MYSQL_USER=root \
-e MYSQL_PASSWORD=secret \
-e MYSQL_DB=todos \
todo-app
