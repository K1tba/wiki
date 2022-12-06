# SQL запросы Elasticsearch

[Документация по SQL запросам в Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/sql-search-api.html)

[[Elasticsearch]] помимо [[REST API Elasticsearch|API]] позволяет обращаться к базе данных посредством **SQL** запросов.

Пример запроса с использованием [[curl]]:

```bash
curl -XPOST "$ES/_sql?format=txt" -H "Content-type: application/json" -d '{"query": "SELECT title FROM notes LIMIT 1"}'
```

Параметр `format=txt` делает вывод читабельным.