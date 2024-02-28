Сервис SNORT

Установка, настройка, запуск сервиса
```
server # apt install snort
```
```
# nano /etc/snort/snort.debian.conf
```
```
...
DEBIAN_SNORT_INTERFACE="eth2"
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
#ls -l /etc/snort/rules
#less /etc/snort/rules/web-iis.rules
```
Тестирование
```
# less /etc/snort/rules/web-iis.rules

# tail -f /var/log/auth.log | grep Red
```

Генерируем атаку

http://192.168.X.10/root.exe

