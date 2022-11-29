# PostgreSQL
### Почитать
- [Документация](https://www.postgresql.org/docs/)
- [Оф. документация на русском](https://postgrespro.ru/docs/postgresql)
- [wiki.postgresql](https://wiki.postgresql.org/wiki/Main_Page/ru)

### [[node-postgres]]

### [[Установка PostgreSQL]]
### [[Управление сервером PostgreSQL]]

### [[Подключение к psql]]

Основная командная оболочка это `psql` - в зависимоти от платформы вызывается по разному (см. ссылки ниже). Причем в Windows для запуска надо находится в директории дистрибутива PostgreSQL. Путь по умолчанию: 
`C:\Program Files\PostgreSQL\14\bin`

> Прим. Перед началом работы с CLI в Windows надо в консоли сменить кодировку выполнив `chcp 1251`

Графическая оболочка `pgAdmin`

Значения по умолчанию:
* пользователь: postgres
* порт: 5432
* пароль при установки на Windows запрашивается установщиком и не может быть пустым (попробуй admin), при установке на [[Ubuntu]] - пароль пустой или я хз какой. В любом случае его лучше установить.

### [[Кодировка psql]]

### [[Установка пароля пользователя PostgreSQL]]
### Создание БД
По умолчанию кодировка новой БД - UTF8. При этом создать просто так БД с другой кодировкой не получится, для этого надо указать шаблон по умолчанию. Пример:
```
CREATE DATABASE foo ENCODING WIN1251 TEMPLATE template0;
```
Для подключения к БД используется такая команда:
```
\c foo;
--или более развёрнуьый вариант
\connect foo;
```

[Поддерживаемые кодировки](https://www.postgresql.org/docs/current/multibyte.html#:~:text=The%20character%20set%20support%20in,8%2C%20and%20Mule%20internal%20code.)

### [[Дамп БД PostgreSQL]]
### [[Создание таблиц PostgreSQL]]
### [[Изменение данных PostgreSQL]]
### Некоторые команды psql
Показать список БД:
`\l`

Показать список таблиц и представлений:
`\d`

Показать список функций:
`\df`

Показать список пользователей:
`\du`

Данные таблицы:
`\d table_name`

Показать кофигурации полнотекстового поиска:
`\dF`

### Турбозагрузка данных
Работает по аналогии с [[Турбо экспорт данных MariaDB|турбо-загрузкой в MariaDB]] и по ощущениям даже быстрее...
```
COPY table_name
FROM 'absolute path to .csv file'
DELIMITER 'delimiter' CSV [HEADER] [ENCODING 'encoding'];
```
Замечание по кодировке. Для задачи по загрузке улиц из [[КЛАДР]] при создании БД надо указать кодировку UTF8. Затем её же нузно указать при импорте CSV:
```
CREATE DATABASE foo ENCODING UTF8 TEMPLATE template0;
--либо просто не указывать кодировку, т.к. по умолчанию UTF8
CREATE DATABASE foo;

CREATE TABLE streets (name VARCHAR, ...);

COPY table_name
FROM 'absolute path to .csv file'
DELIMITER 'delimiter' CSV [HEADER] ENCODING 'UTF8';
```
[Подробнее о команде COPY](https://www.postgresql.org/docs/current/sql-copy.html)

### [[SQL JOINS]]

### TRUNCATE vs DELETE FROM
Trancate очищает таблицу вместе с индексами, статистикой и т.д. Delete только удаляет записи и всё. Поэтому Delete работает быстрее.
[TRUNCATE vs DELETE FROM](https://www.lob.com/blog/truncate-vs-delete-efficiently-clearing-data-from-a-postgres-table)
[Postgresql Truncation speed](https://stackoverflow.com/questions/11419536/postgresql-truncation-speed)

### Константы со спец. символами
Для использования символов типа `\t` в строковых константах, надо писать букву `E` перед самой константой. Пример:
```
SELECT E'fo\to';
```
Экранирование кавычки:
```
SELECT 'foo''bar';
-- эквивалентно
SELECT $$foo'bar$$;
-- эквивалентно
SELECT $Some Tag$foo'bar$Some Tag$;
```
[Подробнее про константы](https://www.postgresql.org/docs/current/sql-syntax-lexical.html#SQL-SYNTAX-CONSTANTS)
### [[Представления]]
### [[Замена ttl-index]]
### [[Полнотекстовый поиск]]
### [[Коды ошибок PostgreSQL]]

#postgreSQL #db