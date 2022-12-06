# Analyze API

[Документация по Analize API](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/indices-analyze.html)
[Почитать про анализатор текста в Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/analysis.html)

Использование анализатора текста:
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