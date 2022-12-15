# Создание индекса Elasticsearch

Индекс в [[Elasticsearch]] это, в терминах [[PostgreSQL]], база данных, которую можно масштабировать горизонтально. При создании можно указать количество шардов и реплик.
При создании индекса,  серверу необходимо передать объект `body` со следующими свойствами:

- `aliases` - _узнать что это..._
- `settings` - настройки индекса, в том числе количество шардов, реплик и т.д.
- `mappings` - настройки полей документов

[Почитать документацию про создание индекса](https://www.elastic.co/guide/en/elasticsearch/reference/8.5/indices-create-index.html).

Если индекс создан, то информация о нём будет представлена в виде такого объекта:
```js
{
	aliases: {...},
	settings: {...},
	mappings: {...},
}
```

###### Пример для [[NodeJS клиент Elasticsearch]]

```typescript
// создать индекс
client.indices.create({
	index: 'index_name',
	body: {
		mappings: {
			properties: {
				title: {
					type: "text",
					analyzer: "russian", // указать анализатор
				}
			}
		}
	}
})
```

Смотри как [[Получить полную информацию об индексе|получить полную информацию об индексе]].


#indices #indexelasticsearch