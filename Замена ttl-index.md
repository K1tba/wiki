# Замена ttl-index

В [[PostgreSQL]] нет ttl-indexes как в [[MongoDB]], но есть обходной путь. Можно создать функцию, которая будет удалять строки с просроченными датами. Вызывать эту функцию всякий раз когда срабатывает тригер:

```
CREATE OR REPLACE FUNCTION expire_del_old_rows() RETURNS trigger
	LANGUAGE plpgsql
	AS $$
BEGIN
	DELETE FROM users WHERE updatedat < NOW() - INTERVAL '30 minute';
	RETURN NEW;
END;
$$;

CREATE OR REPLACE TRIGGER expire_del_old_rows_trigger
AFTER UPDATE OF verificationtoken ON users
EXECUTE PROCEDURE expire_del_old_rows();
```

>Триггерная функция должна возвращать `trigger`

Минус этого подхода в том, что удаление происходит до обновления. Таким образом, если изменяемая строка должна быть удалена, то узнать об этом можно будет только сделав дополнительный SELECT запрос.