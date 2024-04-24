Подключаемся к server

Настравием 
```
nano /etc/ssh/sshd_config
```
```
...
Subsystem sftp internal-sftp
...
Match group user1
       ChrootDirectory %h
       ForceCommand internal-sftp
```
