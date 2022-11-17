# [[MongoDB]] shell (mongosh)

[Документация](https://www.mongodb.com/docs/mongodb-shell/)
[Методы mongosh](https://www.mongodb.com/docs/mongodb-shell/reference/methods/)

#### Данные сервера и БД

Вывести список всех баз данных:
```
show dbs
// или
show databases
```

Получить имя текущей БД можно введя команду
```
db
//или
db.getName()
```

Для получения данных о времени работы и количестве документов в БД:
```
db.stats()
```
Следующая команда возвращает объект с огромным количеством информации о сервере:
```
db.serverStatus()
```

#### Настройка конфигурации mongosh
Можно изменять некоторые [параметры конфигурации](https://www.mongodb.com/docs/mongodb-shell/reference/configure-shell-settings-api/#supported-property-parameters)
```
//установить значение
config.set( "editor", null )

//проверить значение
config.пet( "editor" )

//сбросить до значения по умолчанию
config.куыуе( "editor" )
```

#### Многострочный ввод
Изначально ввод комманд в [[MongoDB]] линейный и не предполагает ввод, состоящий из нескольких строк, наподобии CLI [[PostgreSQL]] или [[MySQL]].
Для перехода в режим многострочного ввода надо запустить люьой редактор введя команду `.editor`. Если ввести её без точки, то будет попытка вызвать внешний редактор и если он не настроен - ошибка.
Для настройки внешнего редактора:
```
config.set( "editor", "nano" )
```
VSCode требует специального флага, поэтому для него:
```
config.set( "editor", "code --wait" )
```
Отключить редактор:
```
config.set( "editor", null )
```
[Подробнее про использование редакторов](https://www.mongodb.com/docs/mongodb-shell/reference/editor-mode/)
#### Создать/удалить БД
Команда создаст новую базу и сразу же переключится на её использование:
```
use newDatabase
```
Для удаления БД:
```
db.dropDatabase()
```

#### Данные коллекции
```
db.collectionName.stats()
```
#### Измение/удаление коллекций
Удалить коллекцию:
```
db.collectionName.drop()
```

Создание коллекции происходит автоматически при добавлении записа в коллекцию. [[Выполнение CRUD операций Mongosh|Смотри CRUD операции]]

#### [[Выполнение CRUD операций Mongosh]]
#### Количество документов в коллекции
```
db.collectionName.documentsCount()
//или
db.collectionName.estimatedDocumentsCount()
```

