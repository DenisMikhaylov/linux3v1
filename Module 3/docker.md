Переходим на сайт docker и производим установку по новой схеме


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
Создание сервиса как в module 1
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
Создание контейнера для приложения с использованием Dockerfile
```
server# mkdir /root/webd/ && cd /root/webd/

server# cp /usr/local/sbin/webd .

server# tar -cvzf www.tgz -C /var/ www/
```
```
server# nano start.sh
```
```
#!/bin/sh

/etc/init.d/inetutils-inetd start

touch /var/log/webd.log
#chown 10003 /var/www/
  
if [ "$MYMODE" = 'TEST' ]; then
  bash      # not work in k8s
else
  tail -f /var/log/webd.log
fi

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
    && echo 'www stream tcp nowait root /usr/local/sbin/webd webd' > /etc/inetd.conf

COPY start.sh /
COPY webd /usr/local/sbin/webd
### ADD www.tgz /var/

### for helm readiness/liveness Probe 
### COPY index.html /var/www/

EXPOSE 80
#ENV MYMODE=TEST

ENTRYPOINT ["/start.sh"]
```
```
# docker build -t test/webd .

# docker history test/webd
```
Запуск в режиме демона и подключение к контейнеру
```
server# docker run --name webd01 --hostname webd01 -itd -v /var/www/:/var/www/ -p 8000:80 test/webd /start.sh
```
