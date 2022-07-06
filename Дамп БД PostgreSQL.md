# Дамп БД PostgreSQL

### Создание дампа
Дамп БД в [[PostgreSQL]] выполняется командой:
```
pg_dump -h hostname -U username -F format -f dumpfile dbname
```
где:
- **hostname** — имя сервера БД;
- **username** — имя пользователя БД (совпадает с именем базы данных);
- **format** — формат дампа (может быть одной из трех букв: 'с' (custom - архив .tar.gz), 't' (tar - tar-файл), 'p' (plain - текстовый файл). В команде букву надо указывать без кавычек.);
- **dumpfile** — имя создаваемого файла дампа;
- **dbname** — имя базы данных.

Пример (имя БД foo):
```
pg_dump -h localhost -U postgres -F c -f mydump.tar.gz foo
```

### Импорт дампа
> Для импорта дампа необходимо чтобы БД существовала

```
pg_restore -h hostname -U username -F format -d dbname dumpfile
```
Параметры аналогичные, за исключением того, что **format** может быть либо 'c', либо 't'.

Пример:
```
pg_restore -h localhost -U postgres -F c -d foo mydump.tar.gz
```

[Почитать про дамп БД](https://help.sweb.ru/entry/113/)