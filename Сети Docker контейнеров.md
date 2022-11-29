# Сети Docker контейнеров

Читать подробнее [про сети в Docker](https://docs.docker.com/network/#network-drivers)

[[Контейнеры Docker]] можно объединять в сеть для того, чтобы они могли обмениваться данными. Посмотреть список всех сетей можно так:
```bash
sudo docker network ls
```

Каждая сеть имеет свой драйвер: `bridge` (драйвер по умолчанию), `host`, `overlay` и т.д.

#### Сеть с автономными контейнерами

[Как использовать мостовую сеть](https://docs.docker.com/network/bridge/)
[Читать про сеть с автономными контейнерами](https://docs.docker.com/network/network-tutorial-standalone/)

Контейнеры можно объединить в сеть, чтобы они могли обращаться друг к другу.  В случае с сетью из автономных контейнеров, можно использовать драйвер `bridge`. Это позволит собрать сеть из контейнеров и обособить их от хост системы.

К примеру надо собрать приложение [[NodeJS]] + [[PostgreSQL]]. Для этого надо использовать  [[Image - образы Docker|официальный образ]] __Postgres__ и кастомный образ __Node__. Вот как это сделать:

Файл для подключения ноды к бд:
```js
const { Pool } = require('pg')

const pool = new Pool({
	user: 'postgres',
	host: process.env.DB_HOST,
	database: 'postgres',
	password: process.env.POSTGRES_PASSWORD,
	port: 5432
})
```

Здесь важно обратить внимание на `host` переменную. Внутри контейнера по адресу `127.0.0.1:5432` не будет базы данных, т.к. она находится в своём автономном контейнере. Чтобы связать контейнер с БД и с нодой надо в контейнер с нодой прокинуть хост на котором висит база. Для этого надо создать сеть и запустить оба образа внутри сети.

Содать сеть:
```bash
sudo docker network create mynetwork

# указать драйвер при создании сети
sudo docker network create --driver bridge mynetwork

# посмотреть список сетей
sudo docker network ls
```

После создания сети, можно взглянуть что там внутри:
```bash
sudo docker network inspect mynetwork

[
    {
        "Name": "mynetwork",
        "Id": "084b35851a9c16655a12fa65bc704850a50c038054fbb7476ed670a995be69cc",
        "Created": "2022-11-29T11:48:57.919466385+03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.22.0.0/16",
                    "Gateway": "172.22.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

Здесь надо обратить внимание на то, что в свойстве `Containers` содержится пустой объект. При создании и запуске контейнеров этот объект будет заполнен.

Запуск контейнера базы данных:
```bash
sudo docker run --rm --network mynetwork -d -p 5435:5432 --name db -e POSTGRES_PASSWORD=admin postgres
```

Здесь надо обратить внимание на 2 вещи:
1. специально указан порт для локальной машины `5435`, т.к. если хост системе запущен [[PostgreSQL]], то скорее всего он уже использует порт по умолчанию;
2. указано имя для контейнера БД `--name db`. Это имя будет использовано контейнером ноды для доступа к хосту базы данных внутри кастомной сети.

Запуск контейнера с приложением:

```bash
sudo docker run --rm --network mynetwork -d -p 3000:3000 --name app -e DB_HOST=db -e POSTGRES_PASSWORD=admin app
```

Теперь, если проинспектировать сеть, то внутри свойства `Containers` будет:
```bash
"Containers": {
            "a1c8f95f6372b0afa90b8c2302e6f9df41f8783411d2b7bd024ab2eba04b22cb": {
                "Name": "app",
                "EndpointID": "e812ef7531abff9c9c643cdaebc6570e27adc01899558eb7b9e2c64c1bc89321",
                "MacAddress": "02:42:ac:16:00:03",
                "IPv4Address": "172.22.0.3/16",
                "IPv6Address": ""
            },
            "ae56215493f5a73be638f76cf0603d2ff7b76255ca9e8bca4515aa6e2a437a40": {
                "Name": "db",
                "EndpointID": "1ba6bfb57361d308acf2f90fe54b797604c5bb82792aff22380f22243aebec80",
                "MacAddress": "02:42:ac:16:00:02",
                "IPv4Address": "172.22.0.2/16",
                "IPv6Address": ""
            }
        },
```

Каждый контейнер получил свой ip адрес внутри сети, также он доступен по имени (внутри сети).

#### Сеть хоста

Можно при запуске контейнеров не делать их автономными, а наоборот сделать так, чтобы они использовали сеть хоста. Таким образом, контейнеры будут использовать порты хост системы. В этом случае, при запуске контейнеров не следует указывать порты, к примеру `-p 5432:5432`

Вариант реализации той же системы, но с хост сетью:
Файл для подключения ноды к бд:
```js
const { Pool } = require('pg')

const pool = new Pool({
	user: 'postgres',
	host: 'localhost',
	database: 'postgres',
	password: process.env.POSTGRES_PASSWORD,
	port: 5432
})
```

Здесь в качестве значения хоста указывается `localhost`, т.к. все контейнеры будут доступны из хост сети.

Запуск контейнеров:

```bash
# контейнер БД
sudo docker run --rm --network host -d --name db -e POSTGRES_PASSWORD=admin postgres

# контейнер с приложением
sudo docker run --rm --network host -d --name app -e POSTGRES_PASSWORD=admin app
```

#network #docker