Переходим на сайт docker и производим установку по новой схеме

Установка Docker

```
apt install docker.io
```

Тестирование установки
```
# systemctl status docker

# docker info

# docker run hello-world
```
Обзор и удаление
```
# docker images

# docker ps -a

# docker container ls -a
```
Создание контейнера для приложения вручную
```
server# docker run -it --name webd --hostname webd debian bash

webd# apt update && apt install file procps nano
```
Создание сервиса inetd как в module 1
Установка ПО INETD 
```
# apt install inetutils-inetd
```
Настройка файла конфигурации
```
# nano /etc/inetd.conf
```
Добавить строку 
```
www stream tcp nowait root /usr/local/sbin/webd webd
```
Запуск сервиса 
```
# service inetutils-inetd restart
```
создание скрипта сервиса
```
# nano /usr/local/sbin/webd
```
вставить
```
#!/bin/bash
base=/var/www
#log=/var/log/webd.log

read request
##echo "$request" >> $log       # for educational demonstration

filename="${request#GET }"
filename="${filename% HTTP/*}"

test $filename = "/" && filename="/index.html"

filename="$base$filename"

while :
do
  read -r header
##  echo "$header" >> $log       # for educational demonstration
  [ "$header" == $'\r' ] && break;
##  [ "$header" == $'' ] && break;    # for STDIN/STDOUT educational demonstration
done

if [ -e "$filename" ]
then
#  echo `date` OK $filename on `hostname` >> $log
  echo -e "HTTP/1.1 200 OK\r"
  echo -e "Content-Type: $(/usr/bin/file -bi \"$filename\")\r"
  echo -e "\r"
  /bin/cat "$filename"
else
#  echo "$(date)" ERR $filename on "$(hostname)" >> $log
  echo -e "HTTP/1.1 404 Not Found\r"
  echo -e "Content-Type: text/html;\r"
  echo -e "\r"
  echo -e "<h1>File $filename Not Found</h1>"
#  ip=$(awk '/32 host/ { print f } {f=$2}' /proc/net/fib_trie | sort -u | grep -v 127.0.0.1)
#  echo -e "Host: $(hostname), IP: $ip, ver 1.1"
fi
```
Настройка прав запуска на скрипт
```
# chmod +x /usr/local/sbin/webd
```
Настройка страницы приветствия для тестов.

```
# mkdir /var/www
```
```
# nano /var/www/index.html
```
```
<html>
<h1>Hello Student</h1>
</html>
```
Создание скрипта запуска
```
webd/# nano start.sh
```
```
#!/bin/sh

/etc/init.d/inetutils-inetd start

bash
```
```
# chmod +x start.sh
```

```
server# docker commit webd test/webd
```
Запуск контейнера в режиме демона
```
docker run --name webd01 --hostname webd01 -itd -p 8000:80 test/webd /start.sh
```
Проверка работы контейнера

```
http://192.168.10.10:8000
```
Анализ контейнера

```
docker inspect webd01
```
```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' webd01 
```
```
docker port webd01
```

Управление контейнером 
```
docker stop webd01
```
```
docker inspect webd01
```
```
 docker start webd01
```
```
http://192.168.10.10:8000/
```
```
 docker stop webd01 && docker rm webd01
```
Создание контейнера для приложения с использованием Dockerfile
```
server# mkdir /root/webd/ && cd /root/webd/
server# cp /usr/local/sbin/webd .
server# cp /var/www/index.html .
```
```
server# nano start.sh
```
```
#!/bin/sh

/etc/init.d/inetutils-inetd start

bash
```
```
chmod +x start.sh
chmod +x webd
```
Создание Dockerfile
```
server# nano Dockerfile
```
```
FROM debian:bullseye

RUN cp /usr/share/zoneinfo/Etc/GMT-3 /etc/localtime \
    && apt-get update \
    && apt-get install -y inetutils-inetd file \
    && apt-get clean \
    && echo 'www stream tcp nowait root /usr/local/sbin/webd webd' > /etc/inetd.conf\
    && mkdir /var/www

COPY start.sh /
COPY webd /usr/local/sbin/webd
COPY index.html /var/www

EXPOSE 80
ENTRYPOINT ["/start.sh"]
```
```
# docker build -t test/webd2 .

# docker history test/webd1
```
Запуск в режиме демона и подключение к контейнеру
```
server# docker run --name webd02 -itd -p 8001:80 --rm -P test/webd2
```
