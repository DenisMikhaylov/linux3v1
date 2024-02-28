TCPWrap
Настройка на Gate

Проверка, что библиотека для ssh используется
```
# ldd /usr/sbin/sshd | grep wrap
```
Настройка
Что разрешено
```
# nano /etc/hosts.allow
```
```
ALL: 127.0.0.0/8
sshd: 192.168.10.10
Sshd: 192.168.10.0/24 192.168.110.0/24
```
что запрещено
```
# nano /etc/hosts.deny
```
```
sshd:ALL
```
