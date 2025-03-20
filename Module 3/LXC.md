LXC


Подключаемся на server

Подготовка родительской (host) системы
```
# apt install bridge-utils

# nano /etc/network/interfaces
```
Заменить настройки сетевого адаптера
```
#eth0 все строки

auto br0
iface br0 inet static
        address 192.168.10.10
        netmask 255.255.255.0
        gateway 192.168.10.1
        bridge_ports eth0
```
или
```
#enp0s3 все строки

auto br0
iface br0 inet static
        address 192.168.10.10
        netmask 255.255.255.0
        gateway 192.168.10.1
        bridge_ports enp0s3
```
```
root@server:~# init 0
```

Для режима bridge в lxc понадобиться включить «неразборчивый режим» в адаптере

Установка и настройка lxc
```
# apt install lxc
```
настройка
```
# nano /etc/default/lxc
```
```
#[ ! -f /etc/default/lxc-net ] || . /etc/default/lxc-net
```
Создание ветки дочерней системы
```
# lxc-create -t debian -n www
```
Установка ПО в дочерней системе
```
# cp /etc/ssh/sshd_config /var/lib/lxc/www/rootfs/etc/ssh/sshd_config

# cp /etc/resolv.conf /var/lib/lxc/www/rootfs/etc/resolv.conf

# chroot /var/lib/lxc/www/rootfs /bin/bash
```
```
root@server:/# PS1='www:\w# '
```
```
www:/# apt install nano vim iputils-ping
```
Настойка сетевых параметров дочерней системы
```
www:/# nano /etc/hosts
```
```
127.0.0.1       localhost

192.168.10.20    www.corp
```
Управление учетными записями в дочерней системе
```
www:/# passwd
... 123

www:/# exit
```
Настройка lxc для запуска дочерней системы в контейнере
```
root@server:~# nano /var/lib/lxc/www/config
```
```
lxc.net.0.type = veth
lxc.net.0.link = br0
lxc.net.0.flags = up
lxc.net.0.ipv4.address = 192.168.10.50/24
lxc.net.0.ipv4.gateway = 192.168.10.1
lxc.start.auto = 1

root@server:~# lxc-ls -f
```
Запуск/мониторинг/остановка контейнера
```
root@server:~# lxc-start -n www 

root@server:~# lxc-info -n www

root@server:~# lxc-attach -n www -- ps ax

root@server:~# lxc-attach -n www -- /bin/bash

root@server:~# ssh 192.168.10.50

root@server:~# lxc-stop -n www

root@server:~# systemctl start lxc@www

root@server:~# systemctl stop lxc@www
```
