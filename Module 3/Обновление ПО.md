2.1 Обновление приложений

DPKG
```
 # less /var/lib/dpkg/status
```
Установленные пакеты
```
# dpkg -l 
```
Содержимое пакета
```
# dpkg -L 
```
В какой пакет входит файл
```
# dpkg -S /etc/init.d/ssh
```
Удаление пакета
```
# dpkg -r firewalld
```
Использование менеджера пакетов APT

Настройка репозитория
```
# cat /etc/apt/sources.list
```
Проверить и добавить текст
```
#deb http://deb.debian.org/debian/ buster main contrib non-free
#deb http://deb.debian.org/debian buster-updates main contrib non-free

deb http://ftp.ru.debian.org/debian/ buster main contrib non-free
deb http://ftp.ru.debian.org/debian/ buster-updates main contrib non-free

deb http://security.debian.org/ buster/updates main contrib non-free

#deb-src http://deb.debian.org/debian buster main contrib non-free
#deb-src http://deb.debian.org/debian buster-updates main contrib non-free
#deb-src http://security.debian.org/ buster/updates main contrib non-free
```
Обновление списка доступных пакетов
```
# apt update
```
Поиск пакета
```
$ apt search antivirus
```
Информация о найденном пакете
```
$ apt show clamav-daemon
```
Какие пакеты зависят от пакета
```
$ apt depends ssh
```
Установка/обновление пакета
```
# apt install clamav-daemon

# DEBIAN_FRONTEND=noninteractive apt -y install postfix
```
Удаление пакета
```
# apt remove snort

# apt autoremove
```
Полное (с конфигами и данными) удаление пакета
```
# apt purge snort
```
Отключение автоматических обновлений
```
# apt purge unattended-upgrades
```
Какие пакеты можно/нужно обновить
```
# apt list --upgradable

# apt-show-versions -i

# apt-show-versions -u

# apt-show-versions -u | grep security

$ ubuntu-support-status --show-all

```
Установка всех обновлений
```
# apt upgrade
```
Поиск пакета (в том числе среди неустановленных) в который входит файл
```
# apt-file update

# apt-file search stddef.h
```
Удаление архива установленных пакетов
```
# apt clean
```
Загрузка пакетов и зависимостей для offline установки
```
# apt install -d zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent

# find /var/cache/apt/archives/

# cd /var/cache/apt/archives/
```
