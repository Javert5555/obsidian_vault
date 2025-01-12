Следующая команда будет запускать клиент MySQL с обычными правами пользователя, и мы получим права администратора внутри базы данных только с помощью аутентификации:

``` bash
mysql -u root -p
```

Если использовать консольный клиент MySQL, можно просто воспользоваться командой-сочетанием клавиш `Ctrl` + `L` для очистки экрана.

Проверьте методы аутентификации, применяемые для каждого из ваших пользователей, чтобы подтвердить, что **root**-пользователь больше не использует для аутентификации плагин `auth_socket`:

``` mysql
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

Создать нового пользователя
``` mysql
CREATE USER 'testUser'@'localhost' IDENTIFIED BY '12345678';
```

Удалить пользователя:
```mysql
DROP USER 'testUser'@'localhost';
```

Изменить пароль пользователя:
```mysql
ALTER USER 'testUser'@'localhost' IDENTIFIED BY 'ABCabc123&';
```

## Предоставление прав пользователю

Команда GRANT используется для предоставления конкретных прав на объекты базы данных. Например, чтобы предоставить пользователю SELECT, INSERT и UPDATE права на все таблицы в определенной базе данных, выполним следующую команду:
```mysql
GRANT SELECT, INSERT, UPDATE ON database_name.* TO 'testUser'@'localhost;
```

Дать пользователю все права на все базы данных:
```mysql
GRANT ALL PRIVILEGES ON *.* TO 'testUser'@'localhost' WITH GRANT OPTION;
```

Дать пользователю все права на конкретную базу данных:
```mysql
GRANT ALL PRIVILEGES ON database_name.* TO 'testUser'@'localhost';
```

Забрать права у пользователя на конкретную таблицу конкретной базы данных:
```mysql
REVOKE SELECT ON database.table FROM 'testUser'@'localhost';
```

Посмотреть права (привилегии) конкретного пользователя:
```mysql
SHOW GRANTS FOR 'testUser'@'localhost';
```

Перезапустить mysql:
```bash
sudo systemctl restart mysql
```

## SSL

```bash
mysql -u root -p -h 127.0.0.1
```

Запросить состояние переменных SSL/TLS:
```mysql
SHOW VARIABLES LIKE '%ssl%';
```

## Решение проблем:

>[!error]
>Failed to fetch http://security.ubuntu.com/ubuntu/pool/main/m/mysql-8.0/mysql-server_8.0.36-0ubuntu0.22.04.1_all.deb  Temporary failure resolving 'ru.archive.ubuntu.com'

Решение:
```bash
sudo mysql -u root -p
```

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
```