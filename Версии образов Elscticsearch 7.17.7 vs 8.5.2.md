# Версии образов Elscticsearch 7.17.7 vs 8.5.2

При использовании [[Image - образы Docker|образов]] [[Elasticsearch]] версий 7.17.7 и 8.5.2 есть некоторые нюансы. В частности, при использовании версии 7.17.7 авторизация не требуется, обращения возможны по протоколу `http://`. В версии 8.5.2 авторизация включена по умолчанию, поэтому при создании контейнера генерируется [[Установить пароль пользователя Elasticsearch|пароль пользователя]], [[Сертификаты безопасности и ключи Elasticsearch|сертификат безопасности]] и т.д.

## 8.5.2

### Создание контейнера

```bash
docker run --rm -t --name db -network darknet -p 9200:9200 -e "discovery.type=single-node" --memory=512m elasticsearch:8.5.2
```

### Подключение к контейнеру

[Подробнее здесь](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker-cli-run-dev-mode)

Для подключения к контейнеру используется [[HTTP]] протокол `https`. При запуске контейнера версии 8.5.2, в логи будет выведена информация о пароле, фингерпринте и т.д. для созданного узла. Пример:

```bash
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Elasticsearch security features have been automatically configured!
✅ Authentication is enabled and cluster connections are encrypted.

ℹ️  Password for the elastic user (reset with `bin/elasticsearch-reset-password -u elastic`):
  Ufhc1HBMh0D+B3sL-Ujq

ℹ️  HTTP CA certificate SHA-256 fingerprint:
  6c3803259d99c3220d217bf4c5e5fbec2852d1e030b98e3fe8e62fdda3453372

ℹ️  Configure Kibana to use this cluster:
• Run Kibana and click the configuration link in the terminal when Kibana starts.
• Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjUuMiIsImFkciI6WyIxNzIuMTguMC4yOjkyMDAiXSwiZmdyIjoiNmMzODAzMjU5ZDk5YzMyMjBkMjE3YmY0YzVlNWZiZWMyODUyZDFlMDMwYjk4ZTNmZThlNjJmZGRhMzQ1MzM3MiIsImtleSI6IlhPUDczSVFCOG5IT0YtOUdydy1lOlJCQWlTdGU0UWNLaTFKVnROeTk3TGcifQ==

ℹ️ Configure other nodes to join this cluster:
• Copy the following enrollment token and start new Elasticsearch nodes with `bin/elasticsearch --enrollment-token <token>` (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjUuMiIsImFkciI6WyIxNzIuMTguMC4yOjkyMDAiXSwiZmdyIjoiNmMzODAzMjU5ZDk5YzMyMjBkMjE3YmY0YzVlNWZiZWMyODUyZDFlMDMwYjk4ZTNmZThlNjJmZGRhMzQ1MzM3MiIsImtleSI6Ilh1UDczSVFCOG5IT0YtOUdyd19XOnNqeXM5UVNUUkgya3B2amtxWGJzSncifQ==

  If you're running in Docker, copy the enrollment token and run:
  `docker run -e "ENROLLMENT_TOKEN=<token>" docker.elastic.co/elasticsearch/elasticsearch:8.5.2`
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Вместе с запросами надо передавать сертификат, сгенерированный при создании [[Контейнеры Docker|контейнера]]. Сертификат  лежит в контейнере здесь: `/usr/share/elasticsearch/config/certs/http_ca.crt`

 Скопировать сертификат на хост машину:

```bash
docker cp <container_name>:/usr/share/elasticsearch/config/certs/http_ca.crt .
```

>Не забудь раздать [[chmod|права на чтение]] файла сертификата.

Подключиться к серверу БД

```bash
curl -X GET --cacert ./http_ca.crt -u elastic "https://localhost:9200"
```

Надо ввести пароль, сгенерированный при запуске контейнера или [[Установить пароль пользователя Elasticsearch|установить свой пароль]].
Пример вывода:
```bash
Enter host password for user 'elastic':
curl: (6) Could not resolve host: Ufhc1HBMh0D+B3sL-Ujq
{
  "name" : "25cf53b3a4c8",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "dmkY778uRA6iYQd2lxLspQ",
  "version" : {
    "number" : "8.5.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "a846182fa16b4ebfcc89aa3c11a11fd5adf3de04",
    "build_date" : "2022-11-17T18:56:17.538630285Z",
    "build_snapshot" : false,
    "lucene_version" : "9.4.1",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}

```

### Использование с NodeJS

Использование [[Elasticsearch]] 8.5.2 с официальным [[NPM|пакетом npm]]

```js
const { Client } = require('@elastic/elasticsearch');
const fs = require('fs');

const client = new Client({
	node: 'https://localhost:9200' ,
	auth: {
		username: 'elastic',
		password: 'Ufhc1HBMh0D+B3sL-Ujq'
	},
	tls: {
		ca: fs.readFileSync('./http_ca.crt'),
		rejectUnauthorized: false
	}
});

(async () => {
	const res = await client.info()
	console.log(res)
})()
```


## 7.17.1

### Создание контейнера

```bash
docker run --rm -t --name db -network darknet -p 9200:9200 -e "discovery.type=single-node" --memory=512m elasticsearch:7.17.7
```

### Подключение к контейнеру

Подключение осуществляется по [[HTTP|http протоколу]]. При запуске контейнера в лог не будет выведен пароль пользователя. Доступ к базе данных:
```bash
curl "http://localhost:9200"
```

Посмотреть какие ноды запущены:
```bash
curl "loclahost:9200/_cat/nodes?v=true&pretty"
```

>**v** - включить подробный вывод
>**pretty** - красивый вывод

### Использование с NodeJS

Использование [[Elasticsearch]] 7.17.7 с официальным [[NPM|пакетом npm]]

```js
const { Client } = require('@elastic/elasticsearch');
const fs = require('fs');

const client = new Client({
	node: 'http://localhost:9200'
});

(async () => {
	const res = await client.info()
	console.log(res)
})()
```

