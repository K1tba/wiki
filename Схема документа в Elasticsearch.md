# Схема документа в Elasticsearch

[Параметры mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-params.html)

Каждый тип в [[Elasticsearch]] имеет свою схему — [mapping](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/glossary.html#glossary-mapping), также как и реляционная таблица. Mapping генерируется автоматически при [[Выполнение CRUD операций Elasticsearch|индексации документа]]:

```bash
# создать запись
curl -X POST "$ES/blog/post/1?pretty" -H "Content-type: application/json" -d '{"name": "GeoS", "weight": 68}'

# посмотреть схему документа
curl -X GET "$ES/blog/_mapping?pretty"
{
  "blog" : {
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "height" : {
          "type" : "long"
        },
        "name" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "weight" : {
          "type" : "long"
        }
      }
    }
  }
}

```


#mapping #schema