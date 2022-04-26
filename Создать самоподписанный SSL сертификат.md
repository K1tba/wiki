# Создать самоподписанный SSL сертификат

```
openssl genrsa -out key.pem
openssl req -new -key key.pem -out csr.pem
openssl x509 -req -days 9999 -in csr.pem -signkey key.pem -out cert.pem
rm csr.pem
```

Это должно оставить вас с двумя файлами, `cert.pem` (сертификат) и `key.pem` (закрытый ключ). Поместите эти файлы в тот же каталог, что и файл вашего сервера Node.js. Это все, что вам нужно для SSL-соединения.

[[Использование https в Koa]]

[источник](https://nodejs.org/en/knowledge/HTTP/servers/how-to-create-a-HTTPS-server/)