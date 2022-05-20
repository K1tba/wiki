# MySQL

[[Лимиты MySQL]]
[[Турбо экспорт данных MariaDB]]

Для запуска сеанса из консоли надо вызвать команду
```
$ mariadb -u user_name -p
```
в MariaDB можно так
```
$ mysql -u user_name -p
```

[MariaDB: учебник для начинающих](https://mariadb.com/kb/ru/a-mariadb-primer/)

Показать все доступные БД
```
SHOW DATABASES;
```

Выбрать БД
```
USE database_name;
```

Показать все доступные таблицы из выбранной БД
```
SHOW TABLES;
```

Вывести текущее время
```
SELECT NOW() as now;
```
