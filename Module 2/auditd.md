---
Практическая работа:
    Название: 'Анализ информационных систем предприятия с точки зрения безопасности'
    Действие: 'Модуль Второй'
---
# **AuditD**
**Время выполнения: 15 минут**

Имена пользователей и пароли:
```
Для denian
login: student 
password: 111
```
```
Для Windows
login: Admin 
password: Pa55w.rd
```
### **Практическая работа**

### **Задача 1: Установка и использование AuditD**

Подключиться по SSH к виртуальной машине gate

Получите права супер пользователя.

```
student@debian:~$ su -
```
```
< Введите пароль пользователя student >
```

Установка и запуск системы аудита

```bash
apt install auditd
```


Настройка правил аудита событий в рамках текущего сеанса ОС



Удаление всех правил
```
auditctl -D
```
Добавление правил в рамках текущего

Самоаудит

Отображение успешных и неудачных попыток чтения информации из записей аудита:
```
auditctl -w /var/log/audit/ -p wra -k auditlog
```
Отображение изменения конфигурации аудита, происходящей во время работы функций журналирования:
```
auditctl -w /etc/audit/ -p wra -k auditconfig
```
```
auditctl -w /etc/libaudit.conf -p wra -k auditconfig
```
Отслеживание использования инструментов управления аудитом:
```
auditctl -w /sbin/auditctl -p x -k audittools
```
```
auditctl -w /sbin/auditd -p x -k audittools
```
```
auditctl -w /usr/sbin/auditd -p x -k audittools
```
```
auditctl -w /usr/sbin/augenrules -p x -k audittoolss
```
Фильтр событий большого объема:
```
 ### Игнорирование событий, связанных с работой shm и lvm 
 
# auditctl -a never,exit -F arch=b32 -F dir=/dev/shm -k sharedmemaccess
# auditctl -a never,exit -F arch=b64 -F dir=/dev/shm -k sharedmemaccess
# auditctl -a never,exit -F arch=b32 -F dir=/var/lock/lvm -k locklvm
# auditctl -a never,exit -F arch=b64 -F dir=/var/lock/lvm -k locklvm
```
Отслеживание изменений параметров Kernel:
```
# auditctl -w /etc/sysctl.conf -p wa -k sysctl
# auditctl -w /etc/sysctl.d -p wa -k sysctl
```

Отслеживание изменения баз пользователей, групп, паролей:
```
# auditctl -w /etc/group -p wa -k etcgroup
# auditctl -w /etc/passwd -p wa -k etcpasswd
# auditctl -w /etc/gshadow -k etcgroup
# auditctl -w /etc/shadow -k etcpasswd
# auditctl -w /etc/security/opasswd -k opasswd
```
Отслеживание изменения файлов sudo:
```
# auditctl -w /etc/sudoers -p wa -k actions
# auditctl -w /etc/sudoers.d/ -p wa -k actions
```
Список всех правил
```
# auditctl -l
```
Добавление правил для постоянного использования
```
# cat /etc/audit/rules.d/audit.rules
```

```
...
-w /etc/sudoers -p wa -k actions
-w /etc/sudoers.d/ -p wa -k actions
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

# ausearch -k passwords-files
```
