# Разбор CORS

[Подробный разбор CORS на ХАБРе](https://habr.com/ru/company/macloud/blog/553826/)

##### Тестовое приложение

Приложение состоит из клиента и сервера на [[NodeJS]]. HTML файл клиента запускается в браузере двойным кликом по файлу.

Backend (index.js):
```js
const Koa = require('koa')

const app = new Koa();

app.use(ctx => {
  console.log(ctx.headers)
  ctx.status = 200;
  ctx.body = 'hello world'
})

app.listen(3333, (error) => {
  if (error) {
    console.log(error.message)
    return;
  }
  console.log('server run at 3333 port')
})
```

Client (page.html):
```
<button onclick=_clicker()>click me</button>
<script>

  function _clicker() {
    fetch('http://localhost:3333/')
      .then(async response => {
        if (response.ok) {
          const res = await response.text();
          console.log(res)
          return;
        }
        throw new Error(`status: ${response.status}`)
      })
      .catch(error => {
        console.log(`error: ${error.message}`)
      })
  }
</script>
```


##### Начало

Источник идентифицируется следующей тройкой параметров: схема, полностью определенное имя хоста и порт.

Если сделать запрос приложения в текущей конфигурации, то браузер заблокирует запрос, а в консоли будет примерно следующее:
```
Запрос из постороннего источника заблокирован: Политика одного источника запрещает чтение удаленного ресурса на [http://localhost:3333/](http://localhost:3333/ "http://localhost:3333/"). (Причина: отсутствует заголовок CORS «Access-Control-Allow-Origin»). Код состояния: 200.
```

При этом сервер всё равно получит обращение со следующими заголовками:
```
{
  host: 'localhost:3333',
  'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0',
  accept: '*/*',
  'accept-language': 'ru-RU,ru;q=0.8,en-US;q=0.5,en;q=0.3',
  'accept-encoding': 'gzip, deflate, br',
  origin: 'null',
  connection: 'keep-alive',
  'sec-fetch-dest': 'empty',
  'sec-fetch-mode': 'cors',
  'sec-fetch-site': 'cross-site'
}
```

> здесь в примере заголовок `origin` имеет значение `null`, т.к. файл был запущен из проводника ОС

*Промежуточный итог: обращение к серверу проходят, но браузер сам блокирует работу с ответом. При этом ответ сервера можно посмотреть в инструментах разработчика. Статус ответа 200.*

##### Методы POST, PATCH, PUT, DELETE

Если отправить запрос методом `POST`, то картина будет такая же как и с методом `GET`. Но, если использовать методы `PATCH`, `PUT`, `DELETE`, то браузер будет отправлять по 2 запроса, а не по 1-му. Первый запрос будет отправлен методом `OPTIONS` без тела. 

Несмотря на то, что сервер ответит статусом 200, тело ответа также будет заблокировано браузером. После отправки запроса `OPTIONS` браузер не будет отправлять оставшийся запрос.

Если клиент при отправке запроса методом `POST` отправляет заголовок `Content-Type` со значением **отличным** от:
- `text/plain`;
- `application/x-www-form-urlencoded`;
- `multipart/form-data`
То, в этом случае браузер также будет отправлять по 2 запроса, где первый будет уходить методом `OPTIONS`. Стоит отметить, что добавление любого кастомного заголовка (напр. 'hi': 'fi') в `POST` запрос также приведёт к отправке 2-х запросов.

> Запрос методом `HEAD` не блокируется [[CORS]]

##### [[Заголовок запроса из безопасного списка CORS]]

##### Исправление для GET и POST

Чтобы `GET` и `POST` запросы не блокировались браузером, сервер должен добавить всего один заголовок в ответ:
```
'Access-Control-Allow-Origin', 'null'
```
> В данном случае значение `null`, т.к. тестовый клиент открыт из проводника ОС. В общем случае сервер должен указать хост клиента.

Если при отправке `GET` и `POST` запросов клиент передаёт какие-то дополнительные заголовки (не из [[Заголовок запроса из безопасного списка CORS|безопасного списка]]), то перед отправкой будет ещё один запрос методом `OPTIONS` и такой запрос будет заблокирован браузером.

Для исправления, сервер должен в ответ добавить заголовок `Access-Control-Allow-Headers`. 
Например, клиент посылает заголовок:
```
'custom-header': 'test'
```

Сервер должен в ответ добавить заголовок:
```
'Access-Control-Allow-Headers', 'custom-header'
```

*Промежуточный итог: обращение к серверу методами `GET` и `POST`, чтобы избежить блокировки сервер в ответе должен передать два заголовка:*
- `Access-Control-Allow-Origin`
- `Access-Control-Allow-Headers`


##### Исправление для PUT, PATCH, DELETE

При использовании этих методов всё тоже самое, что и для `GET` и `POST` запроса (см. выше), за тем исключением, что сервер должен вернуть 3-ий заголовок, указав, какие методы разрешены для обращений:
```
'Access-Control-Allow-Methods', 'PUT, PATCH'
```

##### Отдельный роут для OPTIONS

На сервере можно предусмотреть отдельный роут для запросов методом `OPTIONS`:

```js
router.options('/', (ctx) => {
  ctx.set('Access-Control-Allow-Methods', 'OPTIONS')
  ctx.set('Access-Control-Allow-Headers', 'custom-header')
  ctx.set('Access-Control-Allow-Origin', 'null')

  ctx.status = 200;
  ctx.body = 'hello options'
})

router.all('/', (ctx) => {
  ctx.status = 200;
  ctx.body = 'hello world'
})
```


В этом случае, если браузер будет посылать `OPTIONS` запрос перед другими запросами, то запрос `OPTIONS` будет проходить нормально, а следующий за ним запрос будет заблокирован.


##### Немного про куки

Чтобы разрешить отправку куки с разных источников, нужно чтобы сервер просто вернул заголовок:

```
Access-Control-Allow-Credentials: true
```

#cors 