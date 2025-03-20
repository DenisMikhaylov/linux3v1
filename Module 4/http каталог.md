Создание домашнего каталога на основе HTTP

Подключиться на LAN

Настрйока apache2 для использование домашних каталогов
```
a2enmod userdir
```
```
service apache2 restart
```
Создать пользователя
```
useradd demo1 -m
```
```
chmod 755 /home/demo1
```

Создание каталога
```
mkdir ~demo1/public_html/
```
```
nano ~demo1/public_html/index.html
```
```
<h1>Hello World from demo1</h1>
```
```
chown -R demo1 ~demo1/public_html/
```

Проверка домашнего каталога
```
http://192.168.100.10/~demo1/
```
