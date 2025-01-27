**Нормальная форма** — требование, предъявляемое к структуре таблиц в теории реляционных баз данных для устранения из базы избыточных функциональных зависимостей между атрибутами (полями таблиц).

`C:\Program Files\MySQL\MySQL Server 8.0\bin>mysql -u root -p` 

`SHOW DATABASES;` - отобразить все созданные базы данных
`CREATE DATABASE database_name;` - создать базу данных с именем database_name
`DROP DATABASE database_name;` - удалить базу данных с именем database_name
`USE database_name;` - использовать конкретную бд

`SHOW TABLES;` - посмотреть все таблицы в базе данных

Создание таблицы с полями id и surname:
``` SQL
mysql> CREATE TABLE teacher(
	id INT AUTO_INCREMENT PRIMARY KEY,                                                      surname VARCHAR(255) NOT NULL);
```

``` SQL
CREATE TABLE orders (
	order_id SERIAL PRIMARY KEY,
	customer_id INT,
	product_name VARCHAR(255),
	quantity INT,
	total_amount DECIMAL(10, 2)
);
```
`SHOW TABLES;` - показать все таблицы
`SHOW columns FROM table_name;` - отобразить параметры колонок и их названия

Создаём таблицу с полями: id - уникальным значением, name - строковым значением и внешним ключом (FOREIGN KEY), ссылающимся на колонку id из таблицы teacher:
``` SQL
CREATE TABLE lesson(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(255) NOT NULL,
	teacher_id INT NOT NULL,
	FOREIGN KEY (teacher_id) references teacher(id)
);
```

Удаление таблицы:
``` SQL
DROP TABLE IF EXISTS orders;
```
## Вставка в таблицу

`INSERT INTO table_name (surname) VALUES ("Иванов");` - вставить в таблицу с названием table_name значения: в поле surname (название колонки) значение "Иванов"

`INSERT INTO lesson (name, teacher_id) VALUES ("Математика", 1), ("Информатика", 1), ("Русский", 2), ("Физика", 3);` - вставить в таблицу с названием lesson 4 новых записи (строки), значения которых : для столбца name и столбца teacher_id соответственно ("Математика", 1), ("Информатика", 1), ("Русский", 2), ("Физика", 3)

## Запрос на получение данных
`SELECT * FROM table_name;` - получить значения всех полей из таблицы table_name
`SELECT id, surname FROM teacher;` - получить значения всех полей из указанных колонок таблицы table_name, то есть из колонок id и surname
`SELECT id, surname, id FROM teacher;` - значения из id будут выводиться 2 раза
`SELECT DISTINCT surname FROM teacher;` - получить все уникальные значения поля (колонки) surname из таблицы teacher (без дублирующихся значений)

`SELECT * FROM teacher WHERE id = 1;` - получить все значения полей строк, где значение поля id равно 1

Операторы сравнения в **SQL** приведены в следующей таблице:

![[Pasted image 20230411233147.png]]

`SELECT * FROM teacher WHERE surname = "Петров";` - получить все значения полей строк, где значение поля surname равно значению "Петров" (при сравнении со строковыми, значения оборачиваются в "")

### `LIMIT`
`SELECT * FROM teacher WHERE surname = "Петров" LIMIT 2;` - ограничиваем вывод до первых 2 строк (значений всех полей)

### `OFFSET`
`SELECT * FROM customer OFFSET 2;` - пропускает первые две строки и выводит все остальные (задаёт смещение);

### `AS`
`SELECT id as 'Идентификатор', surname as 'Фамилия' FROM teacher;` - возвращаем значения полей с переименованными названиями колонок(из id в Идентификатор и из surname в Фамилия)

### `COUNT`
`SELECT COUNT(CITY) - COUNT(DISTINCT CITY) FROM STATION;` - вывести разность между количеством всех значений столбца CITY и количеством уникальных значений столбца CITY

## Сортировка данных (выборка) (`ORDER BY`)

`SELECT * FROM teacher ORDER BY surname;` - выбрать все строки (все значения полей всех строк), отсортированные по столбцу surname (по-умолчанию сортирует от меньшего к большему и по алфавиту)

`SELECT * FROM teacher ORDER BY surname DESC;` - выбрать все строки (все значения полей всех строк), отсортированные по столбцу surname в обратном порядке (по убыванию)

`SELECT * FROM teacher ORDER BY id DESC, surname ASC;` -выбирает всех строки из таблицы "teacher", отсортированных по возрастанию по "id" и по убыванию по столбцу "surname"

## Добавление нового поля в существующую таблицу (ALTER TABLE ... ADD)

`ALTER TABLE teacher ADD age INT;` - добавить в таблицу "teacher" новый столбец "age", тип которого - целые числа

## Удаление столбца из таблицы (ALTER TABLE ... DROP)

`ALTER TABLE teacher DROP COLUMN age;` - удаляет из таблицы teacher колонку (со всеми значениями полей) age

## Удаление строк из таблицы

`DELETE FROM teacher WHERE id = 6;` - удалить из таблицы teacher строку(запись) со значением id равного 6

`DELETE FROM teacher` - удалит все строки (записи) из таблицы teacher

## Обновление существующих полей (UPDATE ... SET ... WHERE)

`UPDATE table_name SET age = 20 WHERE id = 1;` - обновили значение поля age (установили новое значение - 20), в строке, id которой равен 1

`UPDATE table_name SET age = 25 WHERE;` - если не задано условие (WHERE), то обновиляются значения всех полей столбца (установили новое значение - 25 для всех полей столбца age)

## Шаблоны SQL

[Шаблоны SQL](https://www.freecodecamp.org/news/sql-contains-string-sql-regex-example-query/) используют операторы LIKE и NOT LIKE и метасимволы (символы, обозначающие что-то отличное от них самих) `%` и `_`.

`SELECT * FROM teacher WHERE surname LIKE "%ов";` - получить все значения полей строк, где значение поля surname соответствует шаблону "%ов" (% - вместо него может быть любое количество символов)

`SELECT * FROM teacher WHERE surname NOT LIKE "%ов";` - получить все значения полей строк, где значение поля surname НЕ соответствует шаблону "%ов" (% - вместо него может быть любое количество символов)

Значение символа:
- `%` - Любая последовательность символов;
- `_` - Ровно один символ;

## Логические операторы (WHERE ... AND/OR/NOT) (BETWEEN)

- Оператор AND отображает запись, если все условия, разделенные символом AND, ВЕРНЫ.
- Оператор OR отображает запись, если какое-либо из условий, разделенных символом OR, является ИСТИННЫМ.
- Оператор NOT отображает запись, если условие (условия) НЕ ВЫПОЛНЯЕТСЯ.
- Оператор BETWEEN выбирает значения в пределах заданного диапазона.

`SELECT * FROM teacher WHERE id > 3 AND surname LIKE "п%ов";` - получить все значения полей строк (все строки), где значение id рано 3 И значение поля surname соответствует шаблону "п%ов"

`SELECT * FROM teacher WHERE id < 3 AND surname NOT LIKE "п%ов";` - получить все значения полей строк (все строки), где значение id меньше 3 И значение поля surname не соответствует шаблону "п%ов"

`SELECT * FROM teacher WHERE NOT id < 3 AND surname NOT LIKE "п%ов" OR age > 30;` - получить все значения полей строк (все строки), где значение id НЕ больше, либо НЕ равно 3 И значение поля surname не соответствует шаблону "п%ов" ИЛИ  значение поля age больше 30

`SELECT * FROM teacher WHERE age BETWEEN 30 AND 45;` - получить все значения полей строк (все строки), где значение поля age больше располагается между 30 и 45 включительно (оба значения).

## SQL Joins (объединение таблиц)

Вот различные типы соединений в SQL:
- `(INNER) JOIN`: возвращает записи, которые имеют совпадающие значения в обеих таблицах.
- `LEFT (OUTER) JOIN`: возвращает все записи из левой таблицы и сопоставленные записи из правой таблицы.
- `RIGHT (OUTER) JOIN`: возвращает все записи из правой таблицы и сопоставленные записи из левой таблицы.
- `FULL (OUTER) JOIN`: возвращает все записи при наличии совпадения в левой или правой таблице.

[Типы соединений которых нет в SQL, но которые можно реализовать самому](https://blog.skillfactory.ru/glossary/join/)

![[Pasted image 20230412183614.png]]

`SELECT teacher.surname, lesson.name FROM teacher INNER JOIN lesson ON teacher.id = lesson.teacher_id;` - получить значения столбцов surname (из ЛЕВОЙ таблицы teacher) и name (из ПРАВОЙ таблицы lesson)  из объединения таблиц (INNER JOIN) по значениям id (из таблицы teacher) и по значениям teacher_id (из таблицы lesson)

`SELECT teacher.surname, lesson.name FROM teacher LEFT OUTER JOIN lesson ON teacher.id = lesson.teacher_id;` - получить значения столбцов surname (из ЛЕВОЙ таблицы teacher) и name (из ПРАВОЙ таблицы lesson)  из объединения таблиц (LEFT OUTER JOIN) по значениям id (из таблицы teacher) и по значениям teacher_id (из таблицы lesson) - (значения lesson.teacher_id, для которых не нашлось соответствия в левой таблицы заменяются на NULL)


`SELECT teacher.surname, lesson.name FROM teacher RIGHT OUTER JOIN lesson ON teacher.id = lesson.teacher_id;` - получить значения столбцов surname (из ЛЕВОЙ таблицы teacher) и name (из ПРАВОЙ таблицы lesson)  из объединения таблиц (RIGHT OUTER JOIN) по значениям id (из таблицы teacher) и по значениям teacher_id (из таблицы lesson) - (значения teacher.id, для которых не нашлось соответствия в правой таблице просто не возвращаются)

## Вертикальное объединение (UNION) (объединение запросов)

Оператор [UNION](https://www.w3schools.com/sql/sql_union.asp) используется для вертикального объединения результатов вызовов двух и более операторов SELECT
Условия для нормальной работы оператора UNION:
- Каждый оператор SELECT в UNION должен иметь одинаковое количество столбцов
- Столбцы также должны иметь схожие типы данных
- Столбцы в каждом операторе SELECT также должны располагаться в том же порядке

`SELECT * FROM teacher UNION SELECT * FROM lesson;` - выводит вертикальное объединение всех записей (из teacher и lesson)

## Агрегатные функции

Агрегатные функции - функции, с помощью которых можно осуществлять какие-либо вычисления.

`SELECT AVG(age) FROM teacher;` - возвращает среднее значение полей столбца age таблицы teacher

`SELECT MAX(age), MIN(age) FROM teacher;` - возвращает максимальное и минимальное значения среди всех полей столбца age таблицы teacher и 

`SELECT SUM(age) FROM teacher;` - возвращает сумму всех значений полей столбца age таблицы teacher и 

`SELECT COUNT(age) FROM teacher;` - возвращает количество  значений полей столбца age таблицы teacher

`SELECT ID FROM table_name WHERE MOD(id, 2) = 1` - выводит все нечётные id

`SELECT ID FROM table_name WHERE MOD(id, 2) = 0` - выводит все чётные id

## Агрегатные функции + Условия для них

`SELECT surname FROM teacher WHERE age = (SELECT MAX(age) FROM teacher);` - выводит значение поля surname из таблицы teacher, из той строки в которой значение поля age является максимальным

`SELECT COUNT(age) FROM teacher WHERE age > 40;` - возвращает количество  значений полей столбца age таблицы teacher, соответствующих заданному условию

## Группировка (GROUP BY)

`SELECT age, COUNT(age) FROM teacher GROUp BY age;` - выводит значения столбца age (без повторения значений) и количество каждого повторения этого значения в столбце (количество человек с соответствующим возрастом)

Оператор GROUP BY часто используется с агрегатными функциями (COUNT(), MAX(), MIN(), SUM(), AVG()) для группировки набора результатов по одному или нескольким столбцам.

SQL-запрос ниже выбирает из таблицы OCCUPATIONS  наиболее часто встречающееся значение столбца Name соответстующее значению столбца Occupation, если имени нет, то выводим NULL, CASE/END позволяет пробегать по условиям when/then/else
``` sql
SELECT MAX(CASE WHEN Occupation = 'Doctor' THEN Name ELSE NULL END) AS Doctor, MAX(CASE WHEN Occupation = 'Professor' THEN Name ELSE NULL END) AS Professor, MAX(CASE WHEN Occupation = 'Singer' THEN Name ELSE NULL END) AS Singer, MAX(CASE WHEN Occupation = 'Actor' THEN Name ELSE NULL END) AS Actor
FROM OCCUPATIONS
GROUP BY Name
ORDER BY Name;
```
https://andreyex.ru/bazy-dannyx/bd-mysql/mysql-pivot-povorot-strok-v-stolbtsy/
! ! ! !Разобраться с агрегирующими функциями! ! ! !

### Запрос с `GROUP BY` и `HAVING`
`HAVING` почти всегда с какой-либо агрегирущей функцией
``` SQL
SELECT column1, column2, SUM(column3) AS sum_column3
FROM table_name
GROUP BY column1, column2
HAVING sum_column3 > threshold;
```


В этом примере запрос выполняет следующие действия:

1. Выбирает столбцы `column1`, `column2`, и `column3` из таблицы `table_name`.
2. Считает сумму значений в столбце `column3` для каждой группы, определенной парами значений `column1` и `column2`.
3. Группирует результаты в соответствии с выбранными столбцами.
4. Фильтрует группы, где сумма значений в столбце `column3` больше заданного порога `threshold`.

Используйте `GROUP BY` и `HAVING` для агрегации данных и фильтрации результатов на основе условий, связанных с агрегированными функциями.

## RIGHT/LEFT

RIGHT(column_name, n)/LEFT(column_name, n) - используется для извлечения последовательности n символов, начиная с правой/левой стороныx`

`SELECT NAME FROM STUDENTS WHERE MARKS > 75 ORDER BY RIGHT(NAME, 3), ID;` - выборка всех имён всех студентов, отсортированных по последним 3 буквам и потом по ID, чьи отметки выше 75


SELECT ROUND(LONG_W, 4) FROM STATION WHERE (LAT_N=MAX(LAT_N) AND LAT_N < 137.2345);

## CASE

Этот запрос использует оператор CASE для проверки трех длин сторон каждой записи в таблице TRIANGLES и вывода соответствующего типа треугольника на основе условий оператора CASE. Если сумма двух меньших сторон меньше или равна наибольшей стороне, то три длины сторон не образуют треугольник, поэтому на выходе получается "Не треугольник". Если все три стороны равны, то треугольник равносторонний, поэтому результат будет "Равносторонний". Если две стороны равны, то треугольник равнобедренный, поэтому на выходе получается "Равнобедренный". Если все три стороны разные, то треугольник является скалярным, поэтому результат будет 'Scalene':

``` SQL
SELECT
  CASE 
    WHEN A+B<=C OR A+C<=B OR B+C<=A THEN 'Not A Triangle'
    WHEN A=B AND B=C THEN 'Equilateral'
    WHEN A=B OR B=C OR A=C THEN 'Isosceles'
    ELSE 'Scalene'
  END AS TriangleType
FROM TRIANGLES;
```

https://metanit.com/sql/mysql/6.4.php

## Concat

`SELECT concat(surname, " ", age) AS surname_age FROM teacher;` - конкантинирует (складывает) несколько значений столбцов друг с другом, со строками, подстроками и так далее

## COALESCE
`COALESCE` позволяет обрабатывать значения NULL, возвращая первое ненулевое значение из списка выражений.
```sql
SELECT product.name, product_photo.url FROM product LEFT JOIN product_photo ON product.id = product_photo.product_id;
```
Результат:
![[Pasted image 20250126223609.png]]

```sql
SELECT COALESCE(product.name, product_photo.url) FROM product LEFT JOIN product_photo ON product.id = product_photo.product_id;
```
Результат:
![[Pasted image 20250126223702.png]]

`COALESCE` можно рассматривать как сокращенную версию оператора CASE. Следующие два выражения эквивалентны:
```sql
SELECT COALESCE(expression1, expression2);
```
аналогично:
```sql
SELECT CASE WHEN expression1 IS NOT NULL THEN expression1 ELSE expression2 END;
```

`COALESCE` может установить значение по умолчанию для колонки (если встретилось поле зо значением `NULL`):
```SQL
SELECT product.name, product_photo.url FROM product LEFT JOIN product_photo ON product.id = product_photo.product_id;
```
Результат:
![[Pasted image 20250126225721.png]]

Добавим `COALESCE`:
```sql
SELECT product.name, COALESCE(product_photo.url, 'empty_url') FROM product LEFT JOIN product_photo ON product.id = product_photo.product_id;
```
Результат:
![[Pasted image 20250126225819.png]]
## SUBSTRING
`SELECT CONCAT(NAME, '(', SUBSTRING(OCCUPATION, 1,1), ')') FROM OCCUPATIONS ORDER BY NAME;` - сконкантинировать значение столбца NAME с первой буквой столбца OCCUPATION обёрнутой в скобки

``` SQL
SELECT CONCAT(NAME, '(', SUBSTRING(OCCUPATION, 1,1), ')') FROM OCCUPATIONS ORDER BY NAME;
SELECT CONCAT('There are a total of ', COUNT(OCCUPATION), ' ', LOWER(OCCUPATION), 's.')
FROM OCCUPATIONS
GROUP BY OCCUPATION
ORDER BY COUNT(OCCUPATION);
```

## Базовые понятия

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

## Regexp SQL
`SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '^[AEIOU]';` - выбрать все уникальные названия городов, которые начинаются на согласные
`SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '[AEIOU]$';` - выбрать все уникальные названия городов, которые заканчиваются на согласные
`SELECT DISTINCT CITY FROM STATION WHERE CITY NOT REGEXP '[AEIOU]$';` - выбрать все уникальные названия городов, которые не заканчиваются на согласные
`SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '^[AEIOU].*[AEIOU]$';` - выбрать все уникальные названия городов, которые заканчиваются и начинаются на согласные
## Функции и триггерные функции

```SQL
CREATE OR REPLACE FUNCTION update_column_value(table_name text, column_name text, new_value int, id_column_name text, id_value int)
RETURNS void AS $$
	BEGIN
		EXECUTE 'UPDATE ' || table_name || ' SET ' || column_name || ' = $1 WHERE ' || id_column_name || ' = $2' USING new_value, id_value;
	END;
$$ LANGUAGE plpgsql;

SELECT update_column_value('actor', 'new_index', 1, 'actor_id', 1);



CREATE OR REPLACE FUNCTION delete_row(table_name TEXT, column_name TEXT, anyelement)
RETURNS void AS $$
	BEGIN
		EXECUTE 'DELETE FROM ' || table_name || ' WHERE ' || column_name || ' = $1' USING $3;
	END;
$$
LANGUAGE plpgsql;

SELECT delete_row('actor', 'actor_id', 205);

-- Создание функции, которая возвращает сумму двух чисел
CREATE FUNCTION add_numbers(x INT, y INT) RETURNS INT AS $$ BEGIN RETURN x + y; END; $$ LANGUAGE plpgsql;

-- Создание функции, которая возвращает таблицу 
CREATE FUNCTION get_customers() RETURNS TABLE (id INT, name VARCHAR(50), email VARCHAR(50)) AS $$ BEGIN RETURN QUERY SELECT id, name, email FROM customers; END; $$ LANGUAGE plpgsql;

```

Триггерная функция:
``` SQL
DROP FUNCTION IF EXISTS concat_with_separators(text, text, text, text, text);

CREATE OR REPLACE FUNCTION concat_with_separators(text, text, text) RETURNS text AS $$
	SELECT $1 || $2 || $3;
$$
LANGUAGE SQL IMMUTABLE;

CREATE AGGREGATE concat_all_str3(text, text) (
sfunc = concat_with_separators,
stype = text,
initcond = ''
);

SELECT concat_all_str3(col1, '_') AS concatenated_values
FROM test_table;
```

