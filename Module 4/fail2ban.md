Установка

```
# apt install fail2ban
```

Настройка для ssh
```
# nano /etc/fail2ban/jail.local
```

```
[sshd]
enabled = true
port     = ssh
filter = sshd
action = iptables[name=sshd, port=ssh, protocol=tcp]
logpath = /var/log/auth.log
maxretry = 5
findtime = 300
bantime = 600
```
Настройка для nginx
```
# nano /etc/fail2ban/jail.local
```
```
[nginx-req-limit]
enabled = true
filter = nginx-req-limit
action = iptables-multiport[name=ReqLimit, port="http,https", protocol=tcp]
logpath = /var/log/nginx/*error.log
findtime = 600
bantime = 7200
maxretry = 10
```
Настройка для asterisk
```
# nano /etc/fail2ban/jail.local
```
```
[asterisk]
enabled = true
action = iptables-allports[name=asterisk, protocol=all]
logpath = /var/log/asterisk/messages
bantime = 86400
```
```
# systemctl restart fail2ban
```
```
# tail -f /var/log/fail2ban.log
```
Мониторинг и управление

```
# fail2ban-client status

# tail -f /var/log/fail2ban.log
```
Для удаления адреса из списка вводим:
```
fail2ban-client set <имя правила> unbanip
```
