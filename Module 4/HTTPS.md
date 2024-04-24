Поддержка протокола HTTPS

На сервере LAN
Настройка Apache
```
# a2enmod ssl
```
```
systemctl restart apache2
```
```
nano /etc/apache2/sites-available/default-ssl*
```
```
...
       SSLCertificateFile    /root/server.crt
       SSLCertificateKeyFile /root/server.key
...
```

```
a2ensite default-ssl
```
```
systemctl restart apache2
```
