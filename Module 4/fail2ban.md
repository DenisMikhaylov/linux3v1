Установка

```
# apt install fail2ban
```

Просмотр настроек
```
nano /etc/fail2ban/jail.conf
```
Просмотр фильтрации настроек SSH
```
nano /etc/fail2ban/filter.d/sshd.conf
```
Проверка включения ssh
```
nano /etc/fail2ban/jail.d/defaults-debian.conf
```
Настройка
```
nano /etc/fail2ban/jail.local
```
```
[sshd]
maxretry = 6 #кол-во попыток
```

```
# systemctl restart fail2ban
```
Мониторинг и управление

```
# fail2ban-client status

# tail -f /var/log/fail2ban.log
```
Для удаления адреса из списка вводим:
```
fail2ban-client set <имя правила> unbanip <ip>
fail2ban-client set sshd unbanip <ip>
```
