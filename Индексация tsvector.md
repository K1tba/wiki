# Индексация tsvector

[Полная информация здесь](https://postgrespro.ru/docs/postgrespro/9.5/textsearch-tables#textsearch-tables-index)


Для ускорения [[Полнотекстовый поиск|полнотекстового поиска]] есть несколько вариантов:

##### Вариант 1: Создание индекса

В этом случае, в таблице создаётся индекс, а при запросе, поисковой движок сам определяет использовать индекс или нет.

Здесь есть 2 тонких момента:
1) при создании индекса, функция `to_tsvector` должна вызываться с 2-мя аргументами, т.е. обязательно надо указать [[Языковая конфигурация|языковую конфигурацию]];
2) при запросе, также надо указывать [[Языковая конфигурация|языковую конфигурацию]] иначе индекс не будет использован.

Пример создания индекса и запроса:
```sql
CREATE INDEX my_table_idx ON my_table USING GIN (to_tsvector('english', body));

SELECT * FROM my_table WHERE to_tsvector('english', body) @@ 'a & b';
```


##### Вариант 2: вынести tsvector в отдельный столбец

В этом случае предпоолагается создание дополнительного столбца типа `tsvector` и запись данных в него. Затем, этот столбец индексируется для ускорения поиска.

Пример из документации:
```sql
ALTER TABLE my_table ADD COLUMN textsearchable_index_col tsvector;

UPDATE my_table SET textsearchable_index_col =
     to_tsvector('english', coalesce(title,'') || ' ' || coalesce(body,''));

CREATE INDEX my_table_idx ON my_table USING GIN (textsearchable_index_col);

SELECT title FROM my_table
	WHERE textsearchable_index_col @@ to_tsquery('create & table');
```

Создание триггера:
[Создание триггеров для обновления данных в tsvector](https://postgrespro.ru/docs/postgrespro/9.5/textsearch-features#textsearch-update-triggers)

```sql
CREATE TRIGGER tsvectorupdate BEFORE INSERT OR UPDATE
	ON my_table FOR EACH ROW EXECUTE PROCEDURE
tsvector_update_trigger(textsearchable_index_col, 'pg_catalog.english', title, body);
```

>Важно: при создании триггера [[Языковая конфигурация|языковую конфигурацию]] нужно указывать полностью, т.е. `pg_catalog.english`, чтобы поведение триггера не менялось при изменениях в пути поиска (`search_path`).


##### Вариант 3: вынести tsvector и языковую конфигурацию в отдельные столбцы

В этом случае должен быть создан дополнительный столбец с типом `regconfig`. Это позволяет использовать разные конфигурации для разных строк.

При написании триггера для такой конфигурации следует использовать функцию: `tsvector_update_trigger_column(_столбец_tsvector_, _столбец_конфигурации_,_столбец_текста_ [, ...])`

##### Вариант 4: использование расширения и установка индекса RUM вместо GIN

Подход описан в [этой статье](https://habr.com/ru/post/443368/). Как выяснилось, чтобы использовать индекс RUM, надо сначала скомпилировать расширение для [[PostgreSQL]] из исходников. На [странице проекта](https://github.com/postgrespro/rum/blob/master/README.md) описывается компиляция под LINUX. Как это сделать для Windows я не разобрался. Поэтому, такой вариант есть, но потестить его не смог.

#tsvector