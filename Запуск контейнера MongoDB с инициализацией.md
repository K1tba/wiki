
# Запуск контейнера MongoDB с инициализацией

###  Создание пользователя

При запуске контейнера  [[MongoDB]] по умолчанию создаётся база данных с названием `admin`. Также можно указать пользователя и пароль. Будет создан пользователь с правами `root`. Пример:

```yaml
db_info:
	image: "mongo"
	environment:
		- MONGO_INITDB_ROOT_USERNAME=root
		- MONGO_INITDB_ROOT_PASSWORD=passwordXXX
```


###  Запуск скриптов инициализации

Для запуска скриптов инициализации надо пробросить папку со скриптами в контейнер, также надо указать переменную окружения `MONGO_INITDB_DATABASE`. Эта переменная указывает на то, с какой базой будут работать скрипты создания БД. Если её не указать, тогда все действия будут выполнены с дефолтной базой данных `test`.

Таким образом, если участь, что при создании пользователя создаётся бд `admin`, то надо писать примерно так:

```yaml
informator:
	image: "geos74/informator:0.0.1"
	ports:
		- "3200:3200"
	environment:
		- SERVER_PORT=3200
		- DB_USER=root
		- DB_PASS=passwordXXX
		- DB_HOST=db_info
		- DB_PORT=27017
		- DB_NAME=admin
	volumes:
		- init-db-informator:/informator/libs/db.init
db_info:
	image: "mongo"
	volumes:
		- init-db-informator:/docker-entrypoint-initdb.d
	environment:
		- MONGO_INITDB_ROOT_USERNAME=root
		- MONGO_INITDB_ROOT_PASSWORD=passwordXXX
		- MONGO_INITDB_DATABASE=admin
```


### Скрипты инициализации

В [документации к образу монги](https://hub.docker.com/_/mongo) сказано, что файлы должны иметь расширения `.js` или `.sh`  и будут выполняться оболочкой [[MongoDB shell (mongosh)]] с базой данных, указанной в переменной `MONGO_INITDB_DATABASE`.

Пример простого скрипта:
```js
db.actions.insertMany([
	{ title: 'Создать' },
	{ title: 'Редактировать' },
]);
```


#mongodb #initdb