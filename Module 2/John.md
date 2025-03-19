---
Практическая работа:
    Название: 'Создание среды для практических работ.'
    Действие: 'Модуль Второй'
---
# **Анализ информационных систем предприятия с точки зрения безопасности**
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

```
# apt install john
$ apt install wget
```

Тест
```
john --test
```

Поиск паролей перебором
```
# john --format=crypt /etc/shadow
```
Поиск паролей по словарю


Скачиваем словарь паролей из интернета
```
# wget https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt
```
Запускаем перебор паролей
```
# john --wordlist=/root/10-million-password-list-top-1000000.txt --format=crypt /etc/shadow
```
