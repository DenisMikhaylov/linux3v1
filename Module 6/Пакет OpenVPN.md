Настройка сертификатов
```
lan# scp /etc/ssl/openssl.cnf gate:/etc/ssl/

lan# scp /var/www/html/ca.crt gate:

lan# scp /var/www/html/ca.crl gate:
```

Создание сертификата сервиса, подписанного CA для gate
```
gate# openssl genrsa -out gate.key 2048
gate# chmod 400 gate.key
```
Создание запроса на сертификат
```
lan# scp /etc/ssl/openssl.cnf gate:/etc/ssl/
```
```
gate# openssl req -new -key gate.key -out gate.req
```
```
...
Common Name (eg, YOUR name) []:gate
...
```
Передача и просмотр содержимого запроса на сертификат
```
gate# scp gate.req lan:

lan# openssl req -text -noout -in gate.req
```
Подпись запроса на сертификат центром сертификации
```
lan# openssl ca -days 365 -in gate.req -out gate.crt
```
```
lan# cat CA/index.txt

lan# ls CA/newcerts/
```
Копирование подписанного сертификата на целевой сервер
```
lan# scp gate.crt gate:

gate# rm gate.req
```
Проверка подписи сертификата
```
gate# wget http://lan/ca.crt

gate# openssl verify -CAfile ca.crt gate.crt
```


Установка сервиса
```
gate# apt install openvpn
```

Генерация файла Диффи-Хелмана
Создайте ключи Диффи-Хелмана следующей командой:
```
# time openssl dhparam -out /etc/openvpn/dh2048.pem 2048
```
Настройка сервера
```
# cp ca.* /etc/ssl/certs/
# cp gate.crt /etc/ssl/certs/
# cp gate.key /etc/ssl/private/
```
```
gate# nano /etc/openvpn/openvpn1.conf
```
```
dev tun
# port 1194
# proto udp
keepalive 10 120
server 192.168.200.0 255.255.255.0
push "route 192.168.110.0 255.255.255.0"

#push "dhcp-option DNS 192.168.X.10"
#push "block-outside-dns"
#push "dhcp-option DOMAIN corpX.un"

dh /etc/openvpn/dh2048.pem

ca /etc/ssl/certs/ca.crt
crl-verify /etc/ssl/certs/ca.crl
cert /etc/ssl/certs/gate.crt
key /etc/ssl/private/gate.key

status /var/log/openvpn1-status.log
```

Тестирование конфигурации
```
# openvpn --config /etc/openvpn/openvpn1.conf

# timeout 5 openvpn --config /etc/openvpn/openvpn1.conf; test $? -eq 124 && echo OK
```
Включение и запуск
```
# systemctl enable openvpn@openvpn1

# systemctl start openvpn@openvpn1
```


Настройка клиента
Windows

```
C:\>notepad C:\Users\student\OpenVPN\config\user1.ovpn
```
```
dev tun
# port 1194
# proto udp
client
remote <ip address gate ext>
ca ca.crt
cert user1.crt
key user1.key
```
или

```
dev tun
port 1194
proto udp
client
remote <ip address gate ext>
ca c:\\ full path с \\ символом ca.crt
cert c:\\ full path с \\ символом user1.crt
key c:\\ full path с \\ символом user1.key
```
