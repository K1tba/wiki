# CORS

[Спецификация CORS](https://fetch.spec.whatwg.org/#http-cors-protocol)

Чтобы обойти блокировку CORS, сервер должен вернуть заголовок:
```
Access-Control-Allow-Origin: 'http://localhost:3000'
```

Это сработает только для `GET` и `POST` запросов. Блокировка останется если клиент использует другие методы, например `PATCH`. Проблема наблюдается в браузерах Chrome и FireFox. 
Интересно, что при использовании метода `PATCH`, браузер посылает не один запрос, а два. Первым улетает запрос `OPTIONS`, а за ним уже `PATCH`. Поэтому если сервер будет возвращать ещё и такой заголовок:

```
Access-Control-Allow-Methods: 'GET, POST, PATCH, DELETE, OPTIONS'
```

то это не решит проблему, хотя ожидается, что это поможет. Для того чтобы всё заработало как надо, сервер на запрос методом `OPTIONS` должен возвращать статус 200.

Вот пример решения для приложения на Koa:

```js
app.use(async (ctx, next) => {
  ctx.set('Access-Control-Allow-Methods', 'GET, POST, PATCH, DELETE, OPTIONS')
  ctx.set('Access-Control-Allow-Origin', 'http://localhost:3000')

  if(ctx.method === 'OPTIONS') {
    return ctx.status = 200;
  }
  await next();
})
```

#cors