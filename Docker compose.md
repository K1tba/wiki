# Docker compose

Пример запуска контейнера [[Использование официальных образов|Postgres]] с инициализацией базы данных `.sql` скриптами. [[NodeJS]] приложение сначала собирается в образ, а затем создается [[Контейнеры Docker|контейнер]]. Можно использовать команду `build` в файле `docker-compose.yml`, тогда [[Image - образы Docker|образ]] будет создан на лету [[файл dockerignore|на основе Dockerfile]]: 
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

### Тома volumes

Для доступа к сриптам инициализации базы данных можно смонтировать каталог с текущего хоста (см. пример выше), а можно использовать `volumes`. Основное отличие в использовании заключается в том, что при монтировании каталога не надо дополнительно указывать директиву `volumes:`, а при использовании томов - надо. Пример `docker-compose.yml`:
```yml
version: '1.0'
services:
  back_app:
    build: .
    volumes:
      - myinitdb:/bimend-test/libs/
    ports:
      - "3500:3500"
    environment:
      - SERVER_PORT=3500
      - DB_USER=bimend
      - DB_HOST=db
      - DB_NAME=bimend
      - DB_PASS=admin
  db:
    image: "postgres"
    volumes:
      - myinitdb:/docker-entrypoint-initdb.d/
    environment:
      - POSTGRES_PASSWORD=admin
      - POSTGRES_USER=bimend
      - POSTGRES_DB=bimend
  front_app:
    image: "geos74/test-task-front"
    ports:
      - "3000:3000"
    environment:
      - SERVER_PORT=3000
      - BACKEND_HOST=localhost
      - BACKEND_PORT=3500
volumes:
  myinitdb:
```


### Сети network

При использовании Docker compose, сервисы включаются в автоматически создаваемую сеть и доступны по своим псевдонимам. В примере выше в контейнер `bridge_app` передаётся переменная `DB_HOST=db`, где `db` - это имя контейнера с базой данных  [[PostgreSQL]]. Также, контейнеры могут обращаться друг к другу по дефолтным портам.



#Dockercompose #compose

### [[Передача булева значения в docker compose]]