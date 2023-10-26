# Использование официального образа redis-stack

[Образ redis-stack-server](https://hub.docker.com/r/redis/redis-stack-server/)

Этот образ подходит для использования с [[NodeJS]] клиентом - [node-redis](https://github.com/redis/node-redis).

Запуск контейнера [[Redis]] с изменённым портом и требованием авторизации:
```bash
docker run -d --name redis-stack -p 6378:6378 --rm -e REDIS_ARGS="--requirepass mypassword --port 6378" redis/redis-stack-server:latest
```

Подключение к контейнеру:
```bash
docker exec -it redis-stack redis-cli -p 6378 -a mypassword
```



#redis-stack