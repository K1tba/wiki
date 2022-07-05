# Создание таблиц PostgreSQL

### Дефолтные значения
При создании таблицы в [[PostgreSQL]] можно указать дефолтное значение для простого упорядоченного идентификатора строк так:
```
CREATE TABLE products (
	id integer DEFAULT nextval('products_id_seq'),
	title text
)
-- или использовать сокращение
CREATE TABLE products (
	id integer SERIAL,
	title text
)
```
При этом если выполнить запись в таблицу с установкой такого поля, то запись будет произведена, но счётчик всё равно не собъется и продолжит увеличиваться.

Пример:
```
INSERT INTO products (title) VALUES ('book');
INSERT INTO products (title) VALUES ('book');
INSERT INTO products (id, title) VALUES (5, 'book');
INSERT INTO products (title) VALUES ('book');
INSERT INTO products (title) VALUES ('book');
INSERT INTO products (title) VALUES ('book');
INSERT INTO products (title) VALUES ('book');
SELECT * FROM products;
```
Выведет:
```
id | title
----------
1  | book
2  | book
5  | book
3  | book
4  | book
5  | book
6  | book
```

Если при создании таблицы для id выставить значение `PRIMARY KEY`, то подобный трюк вызовет ошибку, в момент когда счётчик дойтёт до вставленного ранее значения.

[Установка дефолтных значений](https://www.postgresql.org/docs/current/ddl-default.html)

### Наследование таблиц
```
CREATE TABLE product (title text);
CREATE TABLE music (genre text) INHERITS (product);
SELECT * FROM music;
```
Выведет:
```
title | genre
-------------
      |
(0 rows)
```
Если добавить по записи в обе таблицы и вывести данные, то получится так:
```
INSERT INTO product VALUES ('disc');
INSERT INTO music (title, genre) VALUES ('vinil', 'house');
SELECT * FROM product;

--выведет
title
-----
disc
-----
vinil
```

Чтобы получить только значения из таблицы product надо писать так:
так:
```
SELECT * FROM ONLY product;

--выведет
title
-----
disc
```
