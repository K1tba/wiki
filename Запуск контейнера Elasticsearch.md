# Запуск контейнера Elasticsearch

[Установка  Elasticsearch с Docer-ом](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docker.html#docker-cli-run-dev-mode) 
[[Версии образов Elscticsearch 7.17.7 vs 8.5.2]]

>Важно: при запуске контейнера желательно устанавливать [[Установить ограничение памяти для контейнера|ограничение на использование памяти]]. Иначе, запущенный контейнер быстро "вывешивает" хост систему.

При запуске контейнера [[Image - образы Docker|образ]]  [[Elasticsearch]] должен запускаться с конкретным тегом (версией) `elasticsearch:7.17.7`. Тега `latest` не существует.

Запуск [[Контейнеры Docker|контейнера Docker]]:
```bash
docker run -d --rm --name db --network darknet -p 9200:9200 -e "discovery.type=single-node" --memory=512m elasticsearch:7.17.7
```

Флаг `discovery.type=singlr-node` не указан в руководстве по запуску [[Elasticsearch]] в Docker контейнере. [Узнать больше об этом флаге](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-settings.html)
__discovery.type__ - Указывает, должен ли Elasticsearch формировать кластер из нескольких узлов. По умолчанию `multi-node`, что означает, что Elasticsearch обнаруживает другие узлы при формировании кластера и позволяет другим узлам присоединяться к кластеру позже. Если установлено `single-node`, Эластичный поиск формирует кластер с одним узлом и подавляет тайм-аут, установленный `cluster.publish.timeout`. Для получения дополнительной информации о том, когда вы можете использовать это , см [. Обнаружение одного узла](https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html#single-node-discovery "Обнаружение одного узла") .

Контейнер запускается не очень быстро. Если не отключаться от вывода, указав флаг `-it` или посмотреть логи, то можно увидеть, что при запуске генерируется достаточно большой лог.
[Вот здесь](https://www.elastic.co/guide/en/elasticsearch/reference/current/run-elasticsearch-locally.html) написано, что при запуске [[Elasticsearch]] в лог будет выведен сгенерированный пароль для пользователя `elastic` и токен для __Kibana__. Для версии [[Image - образы Docker|образа]] `elasticsearch:7.17.7` пароль и токен не выводятся в лог.

Для обращения к базе данных, [[Elasticsearch]]  предоставляет [[REST API Elasticsearch|API]], клиентом может быть любая программа, способная отправлять [[HTTP]] запросы. В данном примере используется [[curl|утилита curl]].

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

### [[Версии образов Elscticsearch 7.17.7 vs 8.5.2]]