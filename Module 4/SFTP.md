Подключаемся к server

Настравием 
```
nano /etc/ssh/sshd_config
```
```
...
Subsystem sftp internal-sftp
...
Match group demouser
       ChrootDirectory %h
       ForceCommand internal-sftp
```
Ограничение доступа к сервису SSH

```
nano /etc/ssh/sshd_config
```
```
Match Address 192.168.*.*,172.16.*.*
```

Управление доступом на основе членства в группе
```
nano /etc/ssh/sshd_config
```
```
#AllowGroups sudo

#DenyGroups group1 group2
```
