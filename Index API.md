# Index API

[Документация по Index API](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/indices.html)

Индексы в [[Elasticsearch]] это аналог базы данных в других системах.

##### Проверка существования индекса

Для проверки существования индекса используется `HEAD` [[HTTP|запрос]]. В [[curl]] для отправки запроса методом `HEAD` используется флаг `-I` или `--head`. В случае успеха сервер ответит статусом __200__, или **404** - в противном случае. Пример (проверка существования индекса `notes`):

```bash
curl -I "$ES/notes"

HTTP/1.1 200 OK
X-elastic-product: Elasticsearch
Warning: 299 Elasticsearch-7.17.7-78dcaaa8cee33438b91eca7f5c7f56a70fec9e80 "Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.17/security-minimal-setup.html to enable security."
content-type: application/json; charset=UTF-8
content-length: 645

```

##### Получение индекса

```bash
curl -X GET "$ES/notes?pretty"
```

Если индекс не существует, то сервер вернёт объект с описанием ошибки и статусом **404**. Пример ответа если индекс не найден:
```bash
{
  "error" : {
    "root_cause" : [
      {
        "type" : "index_not_found_exception",
        "reason" : "no such index [notes]",
        "resource.type" : "index_or_alias",
        "resource.id" : "notes",
        "index_uuid" : "_na_",
        "index" : "notes"
      }
    ],
    "type" : "index_not_found_exception",
    "reason" : "no such index [notes]",
    "resource.type" : "index_or_alias",
    "resource.id" : "notes",
    "index_uuid" : "_na_",
    "index" : "notes"
  },
  "status" : 404
}
```

##### Удаление индекса

```bash
curl -X DELETE "$ES/notes?pretty"
```

##### Вывести список всех индексов

Для вывода всех индексов используется команда [[Команда cat|cat]]:

```bash
curl -X GET "http://localhost:9200/_cat/indices?v&pretty"
```
##### [[Создание индекса]]