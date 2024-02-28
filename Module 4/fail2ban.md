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
#ignoreip = 192.168.10.0/24 192.168.110.0/24
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

# tail -f /var/log/fail2ban.log
```
