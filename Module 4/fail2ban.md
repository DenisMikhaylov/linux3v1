Установка

```
# apt install fail2ban
```

Настройка
```
# nano /etc/fail2ban/jail.local
```
```
[ssh]
enabled = true
port     = ssh
filter = sshd
action = iptables[name=sshd, port=ssh, protocol=tcp]
logpath = /var/log/auth.log
maxretry = 5
findtime = 300
bantime = 600
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
