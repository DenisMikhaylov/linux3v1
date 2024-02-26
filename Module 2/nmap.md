
Утилита nmap

Установка
```
# apt install nmap
```
Определение версии системы
```
# nmap -v -A server

# nmap -O server
```
Cканирование TCP портов портов системы
```
# nmap -PN- server

# time nmap -O -p- ya.ru
```
Поиск web серверов в сети
```
$ nmap -p80 192.168.X.7-14
```
Cканирование UDP портов портов системы
```
# nmap -sU server.corpX.un
```
Ping диапазона адресов с verbose и debug
```
# nmap -vvv -ddd -sn -n 195.19.57.1-254
```
