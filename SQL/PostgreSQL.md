
Посмотреть все базы данных:

``` PostgreSQL
SELECT datname FROM pg_database;
-- или
SELECT datname FROM pg_catalog.pg_database;
```


> [!info]
>
Чтобы удалить шаблон базы данных template1 в PostgreSQL, необходимо сбросить флаг pg_database.datistemplate на false. Это можно сделать с помощью команды SQL UPDATE, например:
>
>``` sql
UPDATE pg_database SET datistemplate = false WHERE datname = 'template1';
>```
>
После этого можно удалить базу данных template1 с помощью команды DROP DATABASE:
>
>``` sql
DROP DATABASE template1;
>```



