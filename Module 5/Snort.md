Сервис SNORT

https://www.opennet.ru/base/sec/snort_ids.txt.html

Установка , запуск сервиса
```
server # apt install snort
```
Настройка
```
# nano /etc/snort/snort.debian.conf
```
```
...
DEBIAN_SNORT_INTERFACE="enp0s3"
DEBIAN_SNORT_HOME_NET="192.168.0.0/16"
...
```

```
root@server:~# nano /etc/snort/snort.conf
```

```
...
####################################################################
# Step #6: Configure output plugins
...
output alert_syslog: LOG_AUTH LOG_ALERT
...
```
Настройка

```
root@server:~# snort -T -S HOME_NET=[192.168.0.0/16] -c /etc/snort/snort.conf

root@server:~# service snort restart
```
Создание собственных правил snort

```
#nano /etc/snort/local.rules
```
```
alert tcp any any -> any 80 (msg:"Directory traversal attempt"; flow:to_server; content:"../.."; nocase; reference:url,wiki.val.bmstu.ru; classtype:web-application-attack; sid:1000001; rev:1;)
```
тестирование
```
curl --path-as-is http://server.corpX.un/../../../etc/passwd

```

Просмотр настроек
```
# less /etc/snort/rules/web-iis.rules

# tail -f /var/log/auth.log | grep Red
```


