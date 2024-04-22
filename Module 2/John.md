
Установка John

```
# apt install john
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
# wget https://github.com/danielmiessler/SecLists/blob/master/Passwords/2023-200_most_used_passwords.txt
```
Запускаем перебор паролей
```
# john --wordlist=/root/2023-200_most_used_passwords.txt --format=crypt /etc/shadow
```
