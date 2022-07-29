# Изменение данных PostgreSQL
### Первичный ключ или автоинкрементное поле 
Конструкция типа `id int AUTO_INCREMENT` не работает в [[PostgreSQL]], хотя в [[MySQL]] работает. Такое поле можно добавить так:
```
CREATE TABLE books (id SERIAL, title text);
--или так
CREATE TABLE books (id SERIAL PRIMARY KEY, title text);
```
Простое указание `SERIAL` не делает его первичным ключом.

### Возврат изменённых данных
При вызовет команд на добавление, изменения или удаление данных [[PostgreSQL]] может возвращать измененные данные. Пример:
```
INSERT INTO books (title) VALUES ('GeoS') RETURNING *;
UPDATE books SET title='Honor' WHERE id=1 RETURNING id, title AS main_title;
DELETE FROM books WHERE id=1 RETURNING *;
```
[Почитать про RETURNING](https://www.postgresql.org/docs/current/dml-returning.html)

#returning