# Выполнение CRUD операций Elasticsearch

[Основные операции Elasticsearch](https://habr.com/ru/post/280488/)

[[Elasticsearch]]  - это документоориентированная база данных. В качестве клиента, в примерах ниже, используется утилита  [[curl]].

Предполагается, что [[Запуск контейнера Elasticsearch|контейнер Elasticsearch]] запущен на порту `9200`. 
Для лаконичности будем использовать переменную:
```bash
export ES=localhost:9200

# вывести на экран
echo $ES
```

###  Создание записи

###### v8.5.2:

```bash
curl -X POST --cacert ./http_ca.crt -u elastic "https://localhost:9200/customer/_doc/1?pretty" -H "Content-type: application/json" -d '{"users":{"name": "GeoS"}}' 
```

###### v7.17.7:

```bash
curl -X POST "$ES/blog/post/1?pretty" -H "Content-type: application/json" -d '{"name": "GeoS", "weight": 68}'
```

### Чтение определенной записи

```bash
curl -X GET "$ES/blog/post/1?pretty"
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