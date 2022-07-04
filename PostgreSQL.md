# PostgreSQL
[[Установка PostgreSQL]]

Основная командная оболочка это `psql` - в зависимоти от платформы вызывается по разному (см. ссылки ниже). Причем в Windows для запуска надо находится в директории дистрибутива PostgreSQL. Путь по умолчанию: 
`C:\Program Files\PostgreSQL\14\bin`

> Прим. Перед началом работы с CLI в Windows надо в консоли сменить кодировку выполнив `chcp 1251`

Графическая оболочка `pgAdmin`

Значения по умолчанию:
* пользователь: postgres
* порт: 5432
* пароль при установки на Windows запрашивается усиановщиком и не может быть пустым (попробуй admin), при установке на [[Ubuntu]] - пароль пустой или я хз какой. В любом случае его лучше установить.

Установка пароля для пользователя postgres:
1. Выполнить [[Подключение к psql|подключение к psql]]
2. Выполнить `\password postgres`

[Начало работы в Windows](https://winitpro.ru/index.php/2019/10/25/ustanovka-nastrojka-postgresql-v-windows/)
[Начало работы в Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-ru)

[[Подключение к psql]]

Показать список БД:
`\l`

Показать список таблиц:
`\d`

Показать список пользователей:
`\du`


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