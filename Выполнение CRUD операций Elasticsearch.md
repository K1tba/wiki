# Выполнение CRUD операций Elasticsearch

[API для документов v7.17.7](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docs.html)
[Основные операции Elasticsearch](https://habr.com/ru/post/280488/)
[Использование Elasticsearch](https://losst.pro/ispolzovanie-elasticsearch)

[[Elasticsearch]]  - это документоориентированная база данных. В качестве клиента, в примерах ниже, используется утилита  [[curl]].

В зависимости от версии Elasticsearch обращение к базе данных происходит по разному. [[Версии образов Elscticsearch 7.17.7 vs 8.5.2|Читай про различия версий 7.17.7 и 8.5.2.]]

Предполагается, что [[Запуск контейнера Elasticsearch|контейнер Elasticsearch]] запущен на порту `9200`. 
Для лаконичности будем использовать переменную:
```bash
export ES=localhost:9200

# вывести на экран
echo $ES
```

### Вывести список индесков

###### v 7.17.7
```bash
curl "$ES/_cat/indices?v&pretty"
```

###  Создание одной записи

###### v8.5.2:

```bash
curl -X POST --cacert ./http_ca.crt -u elastic "https://localhost:9200/customer/_doc/1?pretty" -H "Content-type: application/json" -d '{"users":{"name": "GeoS"}}' 
```

###### v7.17.7:

Создать досумент с `id` по умолчанию:
```bash
curl -XPOST "$ES/music/_doc?pretty" -H "Content-type:application/json" -d '{"label":{"rap":"EastSide Records"}}'
```
Создать документ с `id` равным 1:
```bash
curl -XPOST "$ES/music/_doc/1?pretty" -H "Content-type:application/json" -d '{"label":{"jazz":"BoomBaJazz"}}'
```

### Вывести все записи

###### v7.17.7
[Про поиск записей](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/search-search.html)

Вывод всех записей из всех индексов:

```bash
curl "$ES/_search?pretty"

# или так
curl "$ES/*/_search?pretty"

# или так
curl "$ES/_all/search?pretty"
```

Вывод документов определенного индекса:

```bash
curl "$ES/music/_search?pretty"
```

Прочитпть индекс:
```bash
curl "$ES/music/?pretty"
```

### Чтение определенной записи

###### v7.17.7
[Про поиск записей](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/search-search.html)

```bash
curl -X GET "$ES/music/_doc/1?pretty"
```



### Изменение записи
Изменять записи можно [[Методы HTTP|методами POST и PUT]]. Любое изменение документа в [[Elasticsearch]] приводит к тому, что версия документа увеличивается на 1.

```bash
curl -X POST "$ES/blog/post/1?pretty" -H "Content-type: application/json" -d '{"name": "GeoS", "weight": 68}'
```

```bash
curl -X GET "$ES/blog/post/1?pretty"
{
  "_index" : "blog",
  "_type" : "post",
  "_id" : "1",
  "_version" : 4,
  "_seq_no" : 4,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "GeoS"
  }
}

```

### Удаление записи

Удаление документа в [[Elasticsearch]] также приводит к увеличению версии на 1.

```bash
curl -X DELETE "$ES/blog/post/1?pretty"
```


### Удаление индекса

```bash
curl -X DELETE "$ES/<index_name>/?pretty"
```