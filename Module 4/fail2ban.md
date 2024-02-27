Установка

```
# apt install fail2ban
```

Настройка
```
# nano /etc/fail2ban/jail.local
```
```
[sshd]
maxretry = 6
#ignoreip = 192.168.X.0/24 192.168.100+X.0/24
```
```
# service fail2ban reload
```
```
# tail -f /var/log/fail2ban.log
```
Мониторинг и управление
```
# fail2ban-client status

# fail2ban-client status asterisk

# fail2ban-client set asterisk unbanip 172.16.1.150

# tail -f /var/log/fail2ban.log
```
