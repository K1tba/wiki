# Docker compose

Пример запуска контейнера [[Использование официальных образов|Postgres]] с инициализацией базы данных `.sql` скриптами. [[NodeJS]] приложение сначала собирается в образ, а затем создается [[Контейнеры Docker|контейнер]]. Можно использовать команду `build` в файле `docker-compose.yml`, тогда [[Image - образы Docker|образ]] будет создан на лету: 
```yml
version: '1.0'
services:
	db:
		image: "postgres"
		volumes:
			- /home/geos/nodejs/bridge/libs:/docker-entrypoint-initdb.d
		environment:
			- POSTGRES_PASSWORD=admin
			- POSTGRES_USER=bridge
			- POSTGRES_DB=bridge
	bridge_app:
		build: .
		ports:
			- "3500:3500"
		environment:
			- SERVER_PORT=3500
			- DB_USER=bridge
			- DB_HOST=db
```


###  Запуск и остановка контейнеров

[Подробнее про запуск и остановку контейнеров](https://docs.docker.com/compose/gettingstarted/)

Для запуска [[Контейнеры Docker|контейнеров]] используется команда `docker compose up`. Чтобы отключиться от потока вывода следует использовать флаг `-d`.
Для остановки контейнеров можно использовать команды `docker compose stop` или `docker compose down`. Если используется `down`, то контейнеры при остановке удаляются. Флаг `--volumes` почистит [[Volume тома хранения данных|тома]].
Пример команды остановки: 
```bash
docker compose down --volumes
```

#Dockercompose #compose