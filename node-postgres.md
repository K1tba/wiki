# node-postgres

Пакет [[NodeJS]] для работы c [[PostgreSQL]]

Установка: `npm i pg`

[Документация](https://node-postgres.com/)

Пример простого запроса:
```
const { Client, Pool } = require('pg');

const client = new Pool({
  user: 'postgres',
  // host: 'localhost',
  database: 'foo',
  password: 'admin',
  // port: 5432,
});

(async () => {
  // await client.connect()
  const res = await client.query('SELECT $1::text as message', ['Hello world!'])
  console.log(res.rows[0].message) // Hello world!
  await client.end()
})()
```

При использовании `Pool` метод `connect()` можно не вызывать.