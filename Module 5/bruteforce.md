Конфигурация для защиты от bruteforce

Создаем скрипт
```
# cat firewall.sh
```
LOG
```
iptables --flush
...
iptables -A FORWARD -p tcp --dport 80 -i enp0s8 -m conntrack --ctstate NEW -m recent --set
iptables -A FORWARD -p tcp --dport 80 -i enp0s8 -m conntrack --ctstate NEW -m recent --update --seconds 60 --hitcount 4 -j LOG
...
```
DROP
```
iptables -A FORWARD -p tcp --dport 80 -i enp0s8 -m conntrack --ctstate NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
```

```
root@gate:~# tail -f /var/log/syslog

root@gate:~# cat /proc/net/xt_recent/DEFAULT

root@gate:~# echo -10.5.7.1 >/proc/net/xt_recent/DEFAULT

root@gate:~# echo / >/proc/net/xt_recent/DEFAULT
```
Мониторинг соединений

```
root@gate:~# conntrack -L

root@gate:~# iptstate

root@gate:~# conntrack -F
```
