Поддержка протокола HTTPS

На сервере LAN
Настройка Apache
```
# a2enmod ssl
```
```
nano /etc/apache2/sites-available/default-ssl*
```
```
...
       SSLCertificateFile    /root/www.crt
       SSLCertificateKeyFile /root/www.key
...
       # SSLProtocol All -SSLv2 -SSLv3
...
```
```
a2ensite default-ssl
```
```
service apache2 restart
```
