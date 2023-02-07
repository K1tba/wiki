# Использование location в [[nginx]]

Имеем такую структуру папок:
```
html
    │   foo.html
    │   index.html
    │
    ├───img
    │   └───age
    │           pic.jpg
    │
    └───nested
        └───bar
                bar.html
```

Вот такая конфигурация "правильно" отдаёт файлы:
```
server {
  listen 80;
  server_name localhost;

  index  index.html;

  location / {
    root   /usr/share/nginx/html;
    try_files $uri /index.html;
  }

  location /foo/ {
    root   /usr/share/nginx/html;
    try_files $uri /foo.html;
  }

   location /bar/ {
    root   /usr/share/nginx/html/nested;
    index  bar.html;
  }

  location ~* /age/(.+)\.(jpg)$ {
    root   /usr/share/nginx/html/img;
  }
}
```
Запросы:
`http://localhost` вернёт index.html
`http://localhost/any/path/` вернёт index.html

`http://localhost/foo/` вернёт foo.html
`http://localhost/foo/any/path` вернёт foo.html

`http://localhost/bar/` вернёт bar.html
`http://localhost/bar/any/path` вернёт 404-ю ошибку

`http://localhost/age/pic.jpg` вернёт pic.jpg

Дополнительные материалы:

- [Почитать про пути](https://nginx.org/en/docs/http/ngx_http_core_module.html#root)
- [Про алиасы](https://nginx.org/en/docs/http/ngx_http_core_module.html#alias)
- [Масштабируемая архитектура nginx](https://habr.com/ru/company/oleg-bunin/blog/313666/)

#location