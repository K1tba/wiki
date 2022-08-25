# MySQL

[MariaDB: учебник для начинающих](https://mariadb.com/kb/ru/a-mariadb-primer/)

### [[Лимиты MySQL]]
### [[Турбо экспорт данных MariaDB]]

Для запуска сеанса из консоли надо вызвать команду
```
$ mariadb -u user_name -p
```
в MariaDB можно так
```
$ mysql -u user_name -p
```

### Некоторые команды
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



Решение задачи на объединение:
дана таблица
| code | user |
/--------------
| 1    | Mary |
/--------------
| 2    | Alex |
/--------------
| 1    | Ann  |

Чтобы получить всех user-ов одной строкой с одинаковыми кодами можно выполнить следующие:
```
SELECT code, GROUP_CONCAT(user SEPARATOR ', ') FROM table_name GROUP BY code;
```

| code | user |
/--------------
| 1    | Mary, Ann |
/--------------
| 2    | Alex |
/--------------

Более интересный вариант
```
CREATE TEMPORARY TABLE temptable SELECT code, GROUP_CONCAT(user SEPARATOR ', ') AS manager FROM test GROUP BY code;

SELECT * FROM temptable;

UPDATE test T SET full=(SELECT manager FROM temptable M WHERE T.code=M.code);
```

### [[Дамп БД MySQL]]
### [[SQL JOINS]]


#mysql #mariadb