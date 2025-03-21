Использование PKI
Сценарий:

развертывание корпоративного CA (на lan)
в client-ах добавляем сертификат CA в корневые центры сертификации

Установка и запуск сервера Apache на lan
```
# apt install apache2
```
Удаление /var/www/html/index.html
Проверка открытие сайта

Создание центра сертификации

Настройка атрибутов базы CA в конфигурации ssl
```
lan# nano /etc/ssl/openssl.cnf
```
```
...
[ CA_default ]
...
dir           = /root/CA

certificate   = /var/www/html/ca.crt

crl           = /var/www/html/ca.crl

private_key   = $dir/ca.key
...
```
Созданеи структуры PKI
```
cd
mkdir CA
mkdir CA/certs
mkdir CA/newcerts
mkdir CA/crl
touch CA/index.txt
echo "01" > CA/serial
echo "01" > CA/crlnumber
```
Создание зашифрованного приватного ключа
```
lan# openssl genrsa -des3 -out CA/ca.key 2048

Generating RSA key, 2048 bits
Enter PEM pass phrase:Pa$$w0rd
Verifying - Enter PEM pass phrase:Pa$$w0rd
```
Настройка атрибутов организации в конфигурации ssl
```
lan# nano /etc/ssl/openssl.cnf
```
```
...
[ req_distinguished_name ]
...
countryName_default           = RU
stateOrProvinceName_default   = Moscow region
localityName_default          = Moscow
0.organizationName_default    = cko
organizationalUnitName_default = noc
emailAddress_default          = noc@corpX.un

[ req_attributes ]
...
```
Создание самоподписанного корневого сертификата
```
lan# openssl req -new -x509 -days 3650 -key CA/ca.key -out /var/www/html/ca.crt
```
```
Enter pass phrase for ca.key:Pa$$w0rd
...
Common Name (eg, YOUR name) []:corpX.un
```

Инициализация списка отозванных сертификатов

```
lan# openssl ca -gencrl -out /var/www/html/ca.crl
```
```
Enter pass phrase for ./CA/ca.key:Pa$$w0rd
```

Создание сертификата сервиса, подписанного CA

Создание приватного ключа сервиса
```
www# openssl genrsa -out www.key 2048
www# chmod 400 www.key
```
Создание запроса на сертификат
```
lan# scp /etc/ssl/openssl.cnf www:/etc/ssl/
```
```
www# openssl req -new -key www.key -out www.req   #-sha256
```
```
...
Common Name (eg, YOUR name) []:www.corpX.un
...
```
Передача и просмотр содержимого запроса на сертификат
```
www# scp www.req lan:
```
```
lan# openssl req -text -noout -in www.req
```
Подпись запроса на сертификат центром сертификации
```
lan# openssl ca -days 365 -in www.req -out www.crt
```
```
lan# cat CA/index.txt
```
```
lan# ls CA/newcerts/
```
Копирование подписанного сертификата на целевой сервер
```
lan# scp www.crt www:

www# rm www.req
```
Проверка подписи сертификата
```
www# wget http://lan.corpX.un/ca.crt

www# openssl verify -CAfile ca.crt www.crt
```
Проверка соответствия ключа и сертификата
```
$ openssl x509 -noout -modulus -in www.crt | openssl md5

$ openssl rsa -noout -modulus -in www.key | openssl md5
```
Шифрование ключа сервера
```
www# openssl rsa -des3 -in www.clkey -out www.enckey
```

Создание пользовательского сертификата, подписанного CA

Создание приватного ключа пользователя

```
$ openssl genrsa -out user1.key 2048
```
Создание запроса на сертификат
```
$ openssl req -new -key user1.key -out user1.req
```
```
...
Organizational Unit Name (eg, section) [noc]:group1
Common Name (eg, YOUR name) []:user1
Email Address [noc@corpX.un]:user1@corpX.un
...
```
Подпись запроса на сертификат центром сертификации
```
lan# openssl ca -days 365 -in user1.req -out user1.crt

lan# cat CA/index.txt

lan# ls CA/newcerts/
```
Оформление сертификата и ключа в формате PKS#12 с парольной защитой
!!! Сразу импортировать в хранилище сертификатов на клиенте !!!
```
$ openssl pkcs12 -export -in user1.crt -inkey user1.key -out user1.p12 -passout pass:ppassword1

$ openssl pkcs12 -info -in user1.p12
```
Отзыв сертификатов

```
lan# less CA/index.txt

lan# openssl ca -revoke CA/newcerts/02.pem

lan# less CA/index.txt

lan# openssl ca -gencrl -out /var/www/html/ca.crl
```
