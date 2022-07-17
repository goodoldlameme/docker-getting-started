# Задание 1

Собрать и запустить приложение.

# Задание 2

Приложение слушает 3000 порт.
Запустить приложение так, чтобы оно открывалось на localhost:3000 в браузере.
https://docs.docker.com/config/containers/container-networking/#published

Чтобы узнать, на каком порту приложение слушает, нужно его запустить и посмотреть логи.

Что можно использовать, чтобы знать на какому порту выставлено приложение без запуска?

# Задание 3
Поменять в файле app/src/static/js/app.js текст плейсхолдера "New item". 
Перезапустить приложение на том же порту.

# Задание 4
Запушить в docker hub.

# Задание 5
В приложении используется база данных SQLite. 
В ней все данные хранятся в одном локальном файле /etc/todos/todo.db

Сделать так, чтобы после перезапуска данные не исчезали.
https://docs.docker.com/storage/volumes/

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
