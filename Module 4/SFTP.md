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
