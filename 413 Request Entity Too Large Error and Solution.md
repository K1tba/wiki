# 413 Request Entity Too Large Error and Solution

Ошибка возникает если [[Nginx]] получает от клиента слишком большое тело запроса. Исправляется добавлением в [[Конфигурация nginx | конфигурацию]] директивы `client_max_body_size`.

Пример (файл default.conf):

```
client_max_body_size 5m;

server {
  listen 80;
  server_name localhost;

  location / {
    root   /usr/share/nginx/html;
    try_files $uri /index.html;
  }

  location /api/bridge/ {
    proxy_pass http://${HOST_BRIDGE}:${PORT_BRIDGE};
  }
}
```

`client_max_body_size` - Задает максимально допустимый размер тела запроса клиента. Если размер в запросе превышает настроенное значение, 413 (Слишком большой объект запроса) ошибка возвращается клиенту. Имейте в виду, что браузеры не могут корректно отображать эта ошибка. Параметр `client_max_body_size`в 0 отключает проверку клиента запросить размер кузова. Значение по умолчанию - 1m.

#nginx