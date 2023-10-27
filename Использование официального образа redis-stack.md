# Использование официального образа redis-stack

[Образ redis-stack-server](https://hub.docker.com/r/redis/redis-stack-server/)

Этот образ [[Redis]] подходит для использования с [[NodeJS]] клиентом - [node-redis](https://github.com/redis/node-redis).

### Запуск и подключение

Запуск контейнера [[Redis]] с изменённым портом и требованием авторизации:
```bash
docker run -d --name redis-stack -p 6378:6378 --rm -e REDIS_ARGS="--requirepass mypassword --port 6378" redis/redis-stack-server:latest
```

Подключение к контейнеру:
```bash
docker exec -it redis-stack redis-cli -p 6378 -a mypassword
```


### Конфигурация

Дефолтная конфигурация, найденная с помощью [[find]] располагается в директории `/etc/redis-stack.conf` и выглядит так:

```
port 6379
daemonize no
loadmodule /opt/redis-stack/lib/redisearch.so
loadmodule /opt/redis-stack/lib/redistimeseries.so
loadmodule /opt/redis-stack/lib/rejson.so
loadmodule /opt/redis-stack/lib/redisbloom.so
loadmodule /opt/redis-stack/lib/redisgears.so v8-plugin-path /opt/redis-stack/lib/libredisgears_v8_plugin.so
```

[Документация к образу](https://hub.docker.com/r/redis/redis-stack-server/) предлагает прокидывать кастомный конфиг по пути `/redis-stack.conf`.

```bash 
docker run -v ./local-redis-stack.conf:/redis-stack.conf -p 6379:6379 redis/redis-stack-server:latest
```

Попробовал создать простой конфиг с указанием только порта без загрузки модулей  (как в дефолтном конфиге) - это сработало.

> При использовании кастомной конфигурации переменные в docker-compose не подхватываются. Это означает, что установка пароля должна происходить в своей конфигурации


#### Пример конфигурирования

Пример создания контейнера [[Redis]] со своей конфигурацией и дампом базы данных.

redis.conf:
```
port 6378
save 5 2
dbfilename dumper.rdb
dir ./
requirepass mypassword
```

При такой конфигурации дамп будет сохранён по пути `/data/dumper.rdb`. БД будет записано каждые 5 секунд при изменении 2-х и более ключей.

docker-compose.yml:
```yml
version: '1.0'
services:
  redis-stack:
    image: "redis/redis-stack-server:latest"
    ports:
      - "6378:6378"
    volumes:
      - ./redis.conf:/redis-stack.conf
      - ./dump:/data
```

При указании тома для базы данных нельзя указывать имя файла, надо указывать директорию. Если указать имя файла дампа вот так: `- ./dump/dumper.rdb:/data/dumper.rdb`,
то контейнер не запуститься, т.к. при пустом дампе будет ошибка чтения.







### not working

То что ниже можно и нужно не читать, это хлам-код...

```
port 6378
save 5 2
dir ./
dbfilename dumper.rdb
daemonize no
```

```yml
version: '1.0'
services:
  redis-stack:
    image: "redis/redis-stack-server:latest"
    privileged: true
    ports:
      - "6378:6378"
    volumes:
      - ./redis.conf:/redis-stack.conf
      - ./dumper.rdb:/data/dumper.rdb
      - ./init.sh:/init.sh
    environment:
      - REDIS_ARGS="--requirepass=mypassword"
    command: '/bin/sh -c "./init.sh"'
#    command: 'vm.overcommit_memory = 1 | sysctl vm.overcommit_memory=1'
#    sysctls:
#      net.core.somaxconn: "4096"
```


Скрипт взят отсюда: https://r-future.github.io/post/how-to-fix-redis-warnings-with-docker/

```bash
#  WARNING you have Transparent Huge Pages (THP) support enabled in your kernel.
#  This will create latency and memory usage issues with Redis.
#  To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root,
#  and add it to your /etc/rc.local in order to retain the setting after a reboot.
#  Redis must be restarted after THP is disabled.

echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag

# WARNING: The TCP backlog setting of 511 cannot be enforced
# because /proc/sys/net/core/somaxconn is set to the lower value of 128.

sysctl -w net.core.somaxconn=512

# WARNING overcommit_memory is set to 0! Background save may fail under low memory condition.
# To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot
# or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
# The overcommit_memory has 3 options.
# 0, the system kernel check if there is enough memory to be allocated to the process or not,
# if not enough, it will return errors to the process.
# 1, the system kernel is allowed to allocate the whole memory to the process
# no matter what the status of memory is.
# 2, the system kernel is allowed to allocate a memory whose size could be bigger than
# the sum of the size of physical memory and the size of exchange workspace to the process.

sysctl vm.overcommit_memory=1

# start redis server

redis-server /redis-stack.conf --bind 0.0.0.0
```


[Обсуждение проблем](https://stackoverflow.com/questions/43843197/how-to-fix-the-warnings-when-running-the-redisalpine-docker-image)



#redis-stack