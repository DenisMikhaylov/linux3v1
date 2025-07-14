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

Настройка Lan

Настройка системы виртуализации например в Oracle VM VirtualBox

Настрояка сетвых интерфейсов

1. На панели инструментов нажмите кнопку "Настроить".

2. В открывшемся окне слева выберите "Сеть".

3. С правой стороны перейдите на вкладки и настроить выбор согласно списку снизу.:
```
Адаптер 1 - Виртуальный адаптер хоста 
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
auto eер0
iface eth0 inet static
        address 192.168.100.10
        netmask 255.255.255.0
        gateway 192.168.100.1
```
или
```
auto enp0s3
iface enp0s3 inet static
        address 192.168.100.10
        netmask 255.255.255.0
        gateway 192.168.100.1
```
Остальные строчки закоминетировать

13. Изменить имя компьютера
```
hostnamectl set-hostname  lan
```
14. Откройте в текстовом редакторе файл /etc/hosts.

15. Замените вторую строку на
```
127.0.1.1 lan lan.corp.local
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


Установка DHCP на Lan

Проверка и корректировка репозитория.
```
nano /etc/apt/sources.list
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
```
apt update
```
Установка приложения 
```
# apt install isc-dhcp-server
```

Настройка сетевого интерфейса
```
# nano /etc/default/isc-dhcp-server
```
Указывается сетевой интерфейс для раздачи адресов
```
INTERFACESv4="enp0s3"

```

Настройка DHCP 

```
# nano /etc/dhcp/dhcpd.conf
```

Настройка DNS

```
option domain-name "corp.local";
option domain-name-servers <ip address server gate>;
```
Настройка Scope
```
shared-network LAN1 {
  subnet 192.168.100.0 netmask 255.255.255.0 {
    range 192.168.100.100 192.168.100.150;
    option routers 192.168.100.1;
  }
}
```

Проверка работы
```
# dhcpd -t

# service isc-dhcp-server restart

# service isc-dhcp-server status
```

Мониторинг работы DHCP

```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# grep dhcp /var/log/syslog
```

Статитска работы 
```
# apt install dhcpd-pools
# dhcpd-pools
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf

```
