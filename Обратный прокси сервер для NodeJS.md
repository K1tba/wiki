# Обратный прокси сервер для NodeJS

[Nginx в связке с NodeJS](https://gist.github.com/tomasevich/a2fe588c451c5a192893e6521a813020)

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

##### Передача заголовков

В [учебнике от микромягких](https://learn.microsoft.com/ru-ru/troubleshoot/developer/webapps/aspnetcore/practice-troubleshoot-linux/2-2-install-nginx-configure-it-reverse-proxy) предлагается такая конфигурация для прокси-сервера [[Nginx]]:
```
server {
    listen        80;
    server_name _;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

Надо поэксперементировать с `proxy_set_header`