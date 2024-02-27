Установка

```
# apt install portsentry
```
Использование в режиме без блокировки

# cat /etc/portsentry/portsentry.conf
```
BLOCK_UDP="0"
BLOCK_TCP="0"
```
```
# tail -f /var/log/syslog
```
Включение блокировки
```
# cat /etc/portsentry/portsentry.conf
```
```
...
BLOCK_UDP="1"
BLOCK_TCP="1"
...
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
