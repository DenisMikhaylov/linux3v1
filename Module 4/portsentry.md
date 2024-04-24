Установка
Переключаемся на Gate
```
apt install portsentry
```
Тестируем открытые порты
```
ss -tunap
```
Использование в режиме без блокировки
```
nano /etc/portsentry/portsentry.conf
```
```
BLOCK_UDP="0"
BLOCK_TCP="0"
```
Анализ лога
```
tail -f /var/log/syslog
```
На хосте запускаем команду
```
telnet <ip address gate> 143
```
Смотрим что отображает лог


Включение блокировки
```
nano /etc/portsentry/portsentry.conf
```
```
...
BLOCK_UDP="1"
BLOCK_TCP="1"
KILL_ROUTE=/sbin/route add -host $TARGET$ reject
...
```
```
systemctl restart portsentry
```
На хосте запускаем команду
```
telnet <ip address gate> 143
```

На  gate проверяем
```
ip r
```
```
/sbin/route del -host <ip address block> reject
```

Блокировка с использованием libwrap

Включена по умолчанию
Конфигурация в режиме "все разрешено, кроме"
```
# :> /etc/hosts.deny
```
```
# cat /etc/portsentry/portsentry.conf
```
```
...
KILL_HOSTS_DENY="ALL: $TARGET$"
...
```
Просмотр заблокированных хостов
```
# less /var/lib/portsentry/portsentry.blocked.*
```
