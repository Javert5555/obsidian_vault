>[!error]
>Failed to fetch http://security.ubuntu.com/ubuntu/pool/main/m/mysql-8.0/mysql-server_8.0.36-0ubuntu0.22.04.1_all.deb  Temporary failure resolving 'ru.archive.ubuntu.com'

Решение:
`nano /etc/resolv.conf`
Добавить эти строки в этот файл
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```
Сохранить изменения в файле и выполнить следующую команду в терминале:
`apt-get update`
