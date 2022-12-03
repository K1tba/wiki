# Запуск контейнера Elasticsearch

[Установка  Elasticsearch с Docer-ом](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docker.html#docker-cli-run-dev-mode)


>Важно: при запуске контейнера желательно устанавливать [[Установить ограничение памяти для контейнера|ограничение на использование памяти]]. Иначе, запущенный контейнер быстро "вывешивает" хост систему.

При запуске контейнера [[Image - образы Docker|образ]]  [[Elasticsearch]] должен запускаться с конкретным тегом (версией) `elasticsearch:7.17.7`. Тега `latest` не существует.

Запуск [[Контейнеры Docker|контейнера Docker]]:
```bash
docker run -d --rm --name db --network darknet -p 9200:9200 -e "discovery.type=single-node" --memory=512m elasticsearch:7.17.7
```

Контейнер запускается не очень быстро. Если не отключаться от вывода, указав флаг `-it` или посмотреть логи, то можно увидеть, что при запуске генерируется достаточно большой лог.
[Вот здесь](https://www.elastic.co/guide/en/elasticsearch/reference/current/run-elasticsearch-locally.html) написано, что при запуске [[Elasticsearch]] в лог будет выведен сгенерированный пароль для пользователя `elastic` и токен для __Kibana__. Но, где они я так и не нашёл...

Для обращения к базе данных используется [[curl|утилита curl]].

Проверить запуск контейнера [[Elasticsearch]]:
```bash
curl -X GET localhost:9200

{
  "name" : "9a97d32d9062",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "L1ccP5wRT7WpQsiWICrojw",
  "version" : {
    "number" : "7.17.7",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "78dcaaa8cee33438b91eca7f5c7f56a70fec9e80",
    "build_date" : "2022-10-17T15:29:54.167373105Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```