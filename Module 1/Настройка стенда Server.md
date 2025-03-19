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

### **Задача 1: Настройка виртуальных машин**

Настройка Server

Предназначен для HTTP сервисов
Настройка системы виртуализации например в Oracle VM VirtualBox

Настрояка сетвых интерфейсов

1. На панели инструментов нажмите кнопку "Настроить".

2. В открывшемся окне слева выберите "Сеть".

3. С правой стороны перейдите на вкладки и настроить выбор согласно списку снизу.:
```
Адаптер 1 - Внутренняя сеть 
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

auto eth0
iface eth0 inet static
        address 192.168.10.10
        netmask 255.255.255.0
        gateway 192.168.10.1

```
или
```

auto enp0s3
iface enp0s3 inet static
        address 192.168.10.10
        netmask 255.255.255.0
        gateway 192.168.10.1
```
Остальные строчки закоминетировать


13. Изменить имя компьютера
```
hostnamectl set-hostname  server

```
14. Откройте в текстовом редакторе файл /etc/hosts.

15. Замените вторую строку на
```
127.0.1.1 server server.corp.local
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

18.  ОТредактиремай файл Resolve

```
nano /etc/resolv.conf
```
```
nameserver 8.8.8.8
```

20. Проверка связи
```
ping ya.ru
```


Подключаемся по ssh к серверу Server c Gate
```
ssh root@<ip address server >
```
Проверка и корректировка репозитория.
```
#nano /etc/apt/sources.list
```
Если строчка cdrom не закоментирована, удалить ее.
```
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye main

deb http://security.debian.org/debian-security bullseye-security main contrib
deb-src http://security.debian.org/debian-security bullseye-security main contrib

deb http://deb.debian.org/debian/ bullseye-updates main contrib
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib
```
Настройка HTTP на SERVER

Установка ПО INETD 
```
# apt install inetutils-inetd
```
Настройка файла конфигурации
```
# nano /etc/inetd.conf
```
Добавить строку 
```
www stream tcp nowait root /usr/local/sbin/webd webd
```
Запуск сервиса 
```
# service inetutils-inetd restart
```
создание скрипта сервиса
```
# nano /usr/local/sbin/webd
```
вставить
```
#!/bin/bash
base=/var/www
#log=/var/log/webd.log

read request
##echo "$request" >> $log       # for educational demonstration

filename="${request#GET }"
filename="${filename% HTTP/*}"

test $filename = "/" && filename="/index.html"

filename="$base$filename"

while :
do
  read -r header
##  echo "$header" >> $log       # for educational demonstration
  [ "$header" == $'\r' ] && break;
##  [ "$header" == $'' ] && break;    # for STDIN/STDOUT educational demonstration
done

if [ -e "$filename" ]
then
#  echo `date` OK $filename on `hostname` >> $log
  echo -e "HTTP/1.1 200 OK\r"
  echo -e "Content-Type: $(/usr/bin/file -bi \"$filename\")\r"
  echo -e "\r"
  /bin/cat "$filename"
else
#  echo "$(date)" ERR $filename on "$(hostname)" >> $log
  echo -e "HTTP/1.1 404 Not Found\r"
  echo -e "Content-Type: text/html;\r"
  echo -e "\r"
  echo -e "<h1>File $filename Not Found</h1>"
#  ip=$(awk '/32 host/ { print f } {f=$2}' /proc/net/fib_trie | sort -u | grep -v 127.0.0.1)
#  echo -e "Host: $(hostname), IP: $ip, ver 1.1"
fi
```
Настройка прав запуска на скрипт
```
# chmod +x /usr/local/sbin/webd
```
Настройка страницы приветствия для тестов.

```
# mkdir /var/www
```
```
# nano /var/www/index.html
```
```
<html>
<h1>Hello Student</h1>
</html>
```

Добавить маршрут до Server на Хост машине.

```
route add /p 192.168.10.0 mask 255.255.255.0 192.168.100.1
```
