
# Использование официального образа nginx

[Образ nginx на Dockerhub](https://hub.docker.com/_/nginx)

Для запуска контейнера [[Nginx]] надо собрать образ и скопировать статичные файлы в директорию `/usr/share/nginx/html`. Пример **Dockerfile**:

```Dockerfile
FROM nginx
COPY /my/path/to/file /usr/share/nginx/html
```

Запуск контейнера:
```bash
docker build -t some-nginx .

docker run -d --rm --name app -p 8080:80 some-nginx
```

### Использование своей конфигурации

Прежде чем продолжить прочитай про [[Конфигурация nginx | конфигурирование nginx]].

Главный конфигурационный файл находится здесь: `/etc/nginx/nginx.conf`. Выглядит он примерно так:
```
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

events {
  worker_connections  1024;
}

http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;

  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

  access_log  /var/log/nginx/access.log  main;

  sendfile        on;
  #tcp_nopush     on;

  keepalive_timeout  65;
  #gzip  on;

  include /etc/nginx/conf.d/*.conf;
}
```

В данном примере директива `include` подключает все конфигурации из директории `/etc/nginx/conf.d`. Файл конфигурации должен иметь расширение `.conf`. По умолчанию в этой директории есть дефолтный конфиг в файле `default.conf`.
Для того, чтобы прокинуть свою конфигурацию можно в `Dockerfile` указать следующее:
```Dockerfile
FROM nginx
COPY default.conf /etc/nginx/conf.d/default.conf
COPY html /usr/share/nginx/html
```

Т.е. создать файл `default.conf` и при создании образа заменить дефолтную конфигурацию своей.

##### Использование переменных при создании образа

По умолчанию при создании образа на основе nginx нельзя использовать переменные, но есть функция, которая позволяет сделать следующее: указать переменную, которая будет подставлена в шаблон. А затем этот шаблон будет преобразован в конфигурацию. Пример шаблона `default.conf.template` :

```
server {
  listen 80;
  server_name localhost;

  location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
  }

  location /test {
    proxy_pass http://${NODE_HOST}:3000;
  }
}
```

>Идея в том, что при создании образа прокидывается не кастомная конфигурация, а шаблон конфигурации. Переменные можно использовать только в шаблонах.

Файл `Dockerfile`:

```Dockerfile
FROM nginx
COPY default.conf.template /etc/nginx/templates/default.conf.template
COPY html /usr/share/nginx/html
```

Теперь в файле `docker-compose.yml` можно указать переменную `NODE_HOST`:

```yml
version: '1.0'
services:
  front:
    image: "simple-node"
  app:
    build: .
    ports:
      - "8080:80"
    environment:
      - NODE_HOST=front
```

##### Попытка подмены дефолтной конфигурации с помощью volumes

Если при создании образа использовался шаблон конфига, то при попытке запуска контейнера с прокидыванием томов, конфигурация заменена не будет.

Файл `Dockerfile`:

```Dockerfile
FROM nginx
COPY default.conf.template /etc/nginx/templates/default.conf.template
COPY html /usr/share/nginx/html
```

Далее создаём в текущей директории файл `default.conf` с какими-то иными инструкциями и пытаемся запустить образ с помощью [[Docker compose]]

```yml
version: '1.0'
services:
  front:
    image: "simple-node"
  app:
    build: .
    ports:
      - "8080:80"
    environment:
      - NODE_HOST=front
    volumes:
	  - ./default.conf:/etc/nginx/conf.d/default.conf
```

Ничего не произойдёт, контейнер не подхватит файл `default.conf` из хост машины.