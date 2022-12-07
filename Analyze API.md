# Analyze API

[Документация по Analize API](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/indices-analyze.html)
[Почитать про анализатор текста в Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/analysis.html)

###### Использование стандартного анализатора текста
```bash
curl "$ES/_analyze?pretty" -H "Content-type: application/json" -d '{"analyzer":"standard", "text": "hello world"}'

{
  "tokens" : [
    {
      "token" : "hello",
      "start_offset" : 0,
      "end_offset" : 5,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "world",
      "start_offset" : 6,
      "end_offset" : 11,
      "type" : "<ALPHANUM>",
      "position" : 1
    }
  ]
}
```

###### Использование анализатора с указанием языка
Это похоже на то, как работает с разбором текстом [[Типы tsvector и tsquery|tsvector в PostgreSQL]]
```bash
curl "$ES/_analyze?pretty" -H "Content-type: application/json" -d '{"analyzer":"russian", "text": "в холодный зимний вечер, хорошо выпить чашечку кофе"}'
{
  "tokens" : [
    {
      "token" : "холодн",
      "start_offset" : 2,
      "end_offset" : 10,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "зимн",
      "start_offset" : 11,
      "end_offset" : 17,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "вечер",
      "start_offset" : 18,
      "end_offset" : 23,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "вып",
      "start_offset" : 32,
      "end_offset" : 38,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "чашечк",
      "start_offset" : 39,
      "end_offset" : 46,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "коф",
      "start_offset" : 47,
      "end_offset" : 51,
      "type" : "<ALPHANUM>",
      "position" : 7
    }
  ]
}
```