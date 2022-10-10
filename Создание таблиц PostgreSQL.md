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
	id SERIAL,
	title text
)
-- или использовать длинные uid
CREATE EXTENSION IF NOT EXISTS "pgcrypto";
CREATE TABLE users (
  	id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
	title text
);
```

>Если используется `nextval('products_id_seq')`, то сначала надо создать последовательноcть ([подробнее здесь](https://www.postgresql.org/docs/current/sql-createsequence.html)):
>`CREATE SEQUENCE product_id_seq;`
>Читай также про [функции управления последовательностями](https://www.postgresql.org/docs/14/functions-sequence.html).
>Читай про [SERIAL](https://postgrespro.ru/docs/postgresql/9.6/datatype-numeric#datatype-serial)

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

Допускается множественное наследование таблиц.

### Сгенерированные столбцы
Пример:
```
CREATE TABLE books (author text, page int DEFAULT 3, all_pages GENERATED ALWAYS AS (page + 150) STORED);
```

[Подробнее про сгенерированные столбцы](https://www.postgresql.org/docs/current/ddl-generated-columns.html)
### Ограничения
При создании таблиц можно устанавливать ограничения на значения.
Пример сразу 3-х вариантов ограничений:
```
CREATE TABLE books (
	title text, 
	pages int CHECK (pages > 5),
	weight int CONSTRAINT positiveWeight CHECK (weight > 15),
	CHECK (weight > pages)
)
```
[Подробнее про ограничения](https://www.postgresql.org/docs/current/ddl-constraints.html)
Если при записи будут нарушены проверки, то будет сгенерирована ошибка. Если (в данном примере) вес будет ниже 15 - в тексте ошибки будет указан псевдоним проверки `positiveWeight`. 
Порядок выполнения проверок: сначала выполнится проверка `CHECK (weight > pages)`, а затем проверки начнут выполняться "сверху вниз".