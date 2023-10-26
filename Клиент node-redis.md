# Клиент node-redis

node-redis - это [[NodeJS]] клиент для подключения к [[Redis]].

[Документация по node-redis](https://github.com/redis/node-redis)

Для начала использования надо запустить [[Использование официального образа redis-stack|контейнер redis]].

Пример простого использования:
```ts
import { createClient } from 'redis';

const client = await createClient({
  url: 'redis://default:secret_password@my_remote_host:6379'
})
  .on('error', err => console.log('Redis Client Error', err))
  .connect();

await client.set('key', 'value');
const value = await client.get('key');
await client.disconnect();
```

Глядя на этот пример возникают следующие вопросы:
1. нужен ли пул клиентов?
2. надо ли закрывать соединение?

Исходя из [этого обсуждения](https://stackoverflow.com/questions/32383467/redis-connection-pools-node-js) пул клиентов не нужен, поэтому шаблон одно приложение один клиент должен работать правильно. Соединение также можно не закрывать, если оно одно.

Пример реализации работы с одним клиентом. Выносим создание клиента и подключение к [[Redis]] в отдельный модуль (файл `db.ts`):


```ts
import { createClient } from 'redis';

export default createClient({
  url: 'redis://default:secret_password@my_remote_host:6379'
})
.on('error', err => {
  console.log('Redis Client Error', err);
})
.connect();
```


Используем в приложении (файл `index.ts`):

```ts
import db from "./libs/db"

server.on('request', (req, res) => {
  try {
    (async () => {

	  // запрос кэша
      const cache = await (await db).get(req.url || '');

	  // ...
	
	  // кэшируем
      (await db).set(req.url || '', html);
    })();

  } catch (error) {}
});
```

Т.к. из файла библиотеки экспортируется промис, то надо писать именно так: `await (await db).get(req.url || '')`. Двойной await, что может быть лучше :)


#node-redis