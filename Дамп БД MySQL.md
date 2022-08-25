# Дамп БД [[MySQL]]

#### Дамп базы
mysqldump -u [имя пользователя БД] -h localhost -p [название базы] > [путь и название файла дампа с расширением sql]
либо можно не прописывать путь к файлу, а просто зайти в нужную директорию и указать название и расширение дампа

```bash
mysqldump -u root -h localhost -p db_name > /home/dump/fname.sql
```

#### Клон базы
mysqldump -u [имя пользователя БД] -h localhost -p [название базы] | mysql -u [имя пользователя другой БД] -h localhost -p [имя другой БД]

>Прим.: 
> - при клонировании БД нужно будет ввести 2 раза пароль для первой бд и второй бд
> - целевая база данных должна быть создана заранее


```bash
mysqldump -u root -h localhost -p db_name | mysql -u root -h localhost -p new_db_name
```

#### Импорт БД
mysql -h localhost -u [имя пользователя] [имя БД куда импортировать] < [путь откуда импортировать]

```bash
mysql -u root -h localhost -p db_name < /home/dump/fname.sql
```

#dumpmysql