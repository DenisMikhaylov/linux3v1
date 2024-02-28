
Настройка iptable
```
gate:~# cat firewall.sh
```
```
iptables --flush
...
iptables -A FORWARD -p tcp --dport 80 -i eth1 -m conntrack --ctstate NEW -m recent --update --seconds 60 --hitcount 4 -j LOG
iptables -A FORWARD -p tcp --dport 80 -i eth1 -m conntrack --ctstate NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
iptables -A FORWARD -p tcp --dport 80 -i eth1 -m conntrack --ctstate NEW -m recent --set
...
```

root@gate:~# tail -f /var/log/syslog

root@gate:~# cat /proc/net/xt_recent/DEFAULT

root@gate:~# echo -10.5.7.1 >/proc/net/xt_recent/DEFAULT

root@gate:~# echo / >/proc/net/xt_recent/DEFAULT
