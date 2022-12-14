# Создание индекса Elasticsearch

Индекс в [[Elasticsearch]] это, в терминах [[PostgreSQL]], база данных, которую можно масштабировать горизонтально. При создании можно указать количество шардов и реплик.
При создании индекса,  серверу необходимо передать объект `body` со следующими свойствами:

- `aliases` - _узнать что это..._
- `settings` - настройки индекса, в том числе количество шардов, реплик и т.д.
- `mappings` - настройки полей документов

[Почитать документацию про создание индекса](https://www.elastic.co/guide/en/elasticsearch/reference/8.5/indices-create-index.html).

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

// получить данные об индексе
client.indices.get({
	index: 'index_name'
})
```


### Получение данных об индексе

Метод `client.indices.get(...)` возвращает информацию об индексе.

```typescript
// получить данные об индексе
client.indices.get({
	index: 'index_name'
})

// пример вывода:
{
  wiki: {
    aliases: {},
    mappings: { 
	    properties: { 
		    title: {
			    type: 'text', 
			    analyzer: 'russian'
		} },
	},
    settings: {
		index: {
		    routing: { 
			    allocation: { 
				    include: { _tier_preference: 'data_content' } 
			} },
		    number_of_shards: '1',
		    provided_name: 'wiki',
		    creation_date: '1671039634491',
		    number_of_replicas: '1',
		    uuid: 'g5IURK79SNmKRhlqHG5dZw',
		    version: { created: '8050299' }
		}
	}
  }
}
```

#indices #indexelasticsearch