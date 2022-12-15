# Получение информации об Elasticsearch

Для получения информации об [[Elasticsearch]] можно воспользоваться методом `info()`. 
Пример для [[NodeJS клиент Elasticsearch]]:
```typescript
this.client.info()

// вернёт что-то вроде этого
{
    name: '10c121d0b533',
    cluster_name: 'docker-cluster',
    cluster_uuid: 'exl_xL5gQmeeNngGmjkFKA',
    version: {
      number: '7.17.7',
      build_flavor: 'default',
      build_type: 'docker',
      build_hash: '78dcaaa8cee33438b91eca7f5c7f56a70fec9e80',
      build_date: '2022-10-17T15:29:54.167373105Z',
      build_snapshot: false,
      lucene_version: '8.11.1',
      minimum_wire_compatibility_version: '6.8.0',
      minimum_index_compatibility_version: '6.0.0-beta1'
    },
    tagline: 'You Know, for Search'
}
```

Это аналогично простому `GET` запросу к Elasticsearch (в данном примере версии 7.17.7) с помощью [[curl]]:

```bash
curl http://localhost:9200
```