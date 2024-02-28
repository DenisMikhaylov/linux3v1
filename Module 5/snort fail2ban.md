Интеграция fail2ban и snort

Настройка
```
# nano /etc/fail2ban/jail.d/snort_jail.conf
```
```
[snort]
enabled     = true
bantime     = 300
filter      = snort_filter
maxretry    = 3
logpath     = /var/log/auth.log

```
```
# nano /etc/fail2ban/filter.d/snort_filter.conf
```
```
[Definition]

failregex = .*snort.*Priority: 1.*} <HOST>.*
#        .*snort.*Priority: 2.*} <HOST>.*
```
