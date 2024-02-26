Установка и запуск системы аудита

```
# apt install auditd
```

Настройка правил аудита событий

```
# auditctl -D
```
```
# auditctl -w /etc/passwd -p rwa -k passwords-files
# auditctl -w /etc/shadow -p rwa -k passwords-files
```
```
# auditctl -l
```
```
# cat /etc/audit/audit.rules
```

```
...
-w /etc/passwd -p rwa -k passwords-files
-w /etc/shadow -p rwa -k passwords-files
```
```
# service auditd restart
```
Генерация событий
```
user1$ touch /etc/passwd

user1$ cat /etc/shadow
```
Поиск событий
```
# cat /var/log/audit/audit.log

# ausearch -f /etc/passwd
```

# ausearch -k passwords-files
