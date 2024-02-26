
POSIX ACL
```
apt install acl
```
настройка доступа при помощи POSIX
```
# getfacl /etc/passwd

# setfacl -m "user:user1:---" /etc/passwd

# setfacl -x "user:user1:" /etc/passwd

# setfacl -b /etc/passwd
```
