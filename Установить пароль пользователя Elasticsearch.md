# Установить пароль пользователя Elasticsearch

В версии [[Версии образов Elscticsearch 7.17.7 vs 8.5.2|Elasticsearch 8.5.2]] для доступа к базе данных требуетс указать пароль пользователя. Пользователь по умолчанию: `elastic`, а пароль генерируется и выводится в консоль при первом запуске  [[Запуск контейнера Elasticsearch|контейнера Elasticsearch]]. Для сброса пароля надо подключиться к контейнеру и вызвать скрипт `bin/elasticsearch-reset-password`

[Подробнее про флаги  elasticsearch-reset-password](https://www.elastic.co/guide/en/elasticsearch/reference/current/reset-password.html#reset-password-parameters)

##### Генерация нового пароля

```bash
docker exec -it db /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic -i
```

##### Установка своего пароля


