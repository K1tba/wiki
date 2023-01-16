# Обратный прокси сервер для NodeJS

Для создания обратного прокси сервера [[Nginx]] надо указать в файле конфигурации `location` с адресом. При обращении на этот адрес, обращение будет перенаправляться на сервер [[NodeJS]]. Пример:

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