# Выполнение CRUD операций [[MongoDB shell (mongosh)|mongosh]]

[Описание команд](https://www.mongodb.com/docs/mongodb-shell/crud/)

#### Добавить документ в коллекцию
```
db.collectionsName.insertOne({
	...data
})

db.collectionsName.insertMany([
	{
		...data
	},
	{
		...data
	}
])
```

Если ошибиться в названии коллекции, то будет создана новая коллекция и запись попадёт в неё.
#### Изменить документ коллекции
```
//Изменит один документ
db.collectionsName.updateOne(
	{title: "foo"},
	{
		$set: {
			age: 18,
			name: 'Alex'
		}
	}
)

//Изменит все документы удовлетворяющие условию
db.collectionsName.updateMany(
	{title: "foo"},
	{
		$set: {
			age: 18,
			name: 'Alex'
		}
	}
)
```

Для замены документа:
```
db.collectionsName.replaceOne(
	{title: "foo"},
	{
		$set: {
			age: 18,
			name: 'Alex'
		}
	}
)
```
>Операция replaceOne заменяет полностью все поля документа, за исключением `_id`, это поле остаётся без изменений
#### Удаление документа
```
//Удалит один документ
db.collectionsName.deleteOne( {title: "foo"} )

//Удалит все документы удовлетворяющие условию
db.collectionsName.deleteMany( {title: "foo"} )
```
