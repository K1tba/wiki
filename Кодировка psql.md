# Кодировка psql

Использование `SET client_encoding TO`. Клиентская кодировка устанавливается следующей SQL-командой:
```
SET CLIENT_ENCODING TO '_value_';
```


Также, для этой цели можно использовать стандартный синтаксис SQL `SET NAMES`:
```
SET NAMES '_value_';
```


Получить текущую клиентскую кодировку:

```
SHOW client_encoding;
```

Вернуть кодировку по умолчанию:

```
RESET client_encoding;
```

[Подробнее здесь](https://postgrespro.ru/docs/postgresql/9.6/multibyte#idp76)
