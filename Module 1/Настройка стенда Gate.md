---
Практическая работа:
    Название: 'Создание среды для практических работ.'
    Действие: 'Модуль первый'
---
# **Создание сервисов и сети предприятия**
**Время выполнения:  минут**

В данной практической работе мы создадим стенд в рамках, которого будем выполнять наш курс .

Этапы создания стенда:

- Импорт виртуальных машин

- Настройка виртуальных машин и сети

- Развертывания сервисов

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

### **Задача 1: Импорт виртуальных машин**
Импортировать 4 виртуальные машины
```
3 Debian
Gate 
Lan
Server
```
```
1 Windows
Win
```

### **Задача 2: Настройка виртуальных машин**

Gate предназначен для пограничного маршрутизатора и межсетевого экрана

Настройка Gate

Настройка системы виртуализации например в Oracle VM VirtualBox

Настрояка сетвых интерфейсов

1. На панели инструментов нажмите кнопку "Настроить".

2. В открывшемся окне слева выберите "Сеть".

3. С правой стороны перейдите на вкладки и настроить выбор согласно списку снизу.:
```
Адаптер 1 - Внутренняя сеть
Адаптер 2 - Сетевой мост 
Адаптер 3 - Виртуальный адаптер хоста 
```
4. На каждом адаптере нажмите на надпись "Дополнительно".

5. Нажать на кнопку с двумя синими стрелочками и перегенерировать MAC-адрес

6. Нажмите кнопку "OK".
   
8. На панели инструментов нажмите кнопку "Запустить".

9. Войдите в систему под учетной записью указанной сверху.

10. Запустите терминул.

11. Получите права супер пользователя.

```
student@debian:~$ su -
```
```
< Введите пароль пользователя student >
```

12. Настроить сетевые адреса
    
```
# nano /etc/network/interfaces
```
```
# DMZ
auto eth0
iface eth0 inet static
address 192.168.10.1/24

#WAN
auto eth1
iface eth1 inet dhcp

#LAN
auto eth2
iface eth2 inet static
address 192.168.100.1/24        
```
or
```
# DMZ
auto enp0s3
iface enp0s3 inet static
address 192.168.10.1/24

#WAN
auto enp0s8
iface enp0s8 inet dhcp

#LAN
auto enp0s9
iface enp0s9 inet static
address 192.168.100.1/24  
```
13. Изменить имя компьютера
```
hostnamectl set-hostname  gate
```
14. Откройте в текстовом редакторе файл /etc/hosts.

15. Замените вторую строку на
```
127.0.1.1 gate gate.corp.local
```

16. Перезапустить сетевые адаптеры
```
systemctl restart networking
```
17. Проверка IP адресации
```
ip a
ip r
```
18. Проверка связи
```
ping ya.ru
```


Подключаемся по ssh к серверу Gate под студентом

1. Получите права супер пользователя.

```
student@debian:~$ su -
```
```
< Введите пароль пользователя student >
```


2. Настройка маршрутизации

Настраиваем перенаправление ip
```
nano /etc/sysctl.conf
```
Правим строку
```
net.ipv4.ip_forward=1
```
Перечитаем конфигурацию
```
sysctl -f
```
Проверяем
```
sysctl net.ipv4.ip_forward
```
Проверка таблицы маршрутов
```
ip r
```

```
# sysctl -w net.ipv4.ip_forward=1
```
3. Установка По для маршрутизации

4. Проверка репозиториев

```
nano /etc/apt/sources.list
```
```
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye main

deb http://security.debian.org/debian-security bullseye-security main contrib
deb-src http://security.debian.org/debian-security bullseye-security main contrib

deb http://deb.debian.org/debian/ bullseye-updates main contrib
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib
```
```
apt update
```
```
# apt install iptables
```
```
# apt install conntrack
```
```
# nano gatenat.sh
```
```
iptables -t nat --flush
iptables -t nat -A POSTROUTING -o enp0s8 -s 192.168.10.0/24 -j MASQUERADE
iptables -t nat -A POSTROUTING -o enp0s8 -s 192.168.100.0/24 -j MASQUERADE
conntrack -F
```
Запуск скрипта

```
# chmod +x gatenat.sh
# ./gatenat.sh
```
Проверка

```
#iptables-save
```
Сохранение

```
iptables-save > /etc/iptables.rules
```
Изминенеи настроек 
```
# nano /etc/network/interfaces
```
добавление строчки в конец файла
```
pre-up iptables-restore < /etc/iptables.rules 
```

