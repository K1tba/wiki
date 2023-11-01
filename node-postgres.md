# node-postgres

Пакет [[NodeJS]] для работы c [[PostgreSQL]]

Установка: `npm i pg`

[Документация](https://node-postgres.com/)
[Описание ошибок](https://www.postgresql.org/docs/current/errcodes-appendix.html)

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

> При использовании `Pool` метод `connect()` можно не вызывать.



### Проверенный вариант

Использует пул соединений, причём обязательно надо слушать событие `error` на пуле, иначе если коннект с [[PostgreSQL|базой]] прервётся программа будет аварийно завершена. Также, в функции `query` использован подход из документации: извлечение клиента, запрос, освобождение клиента. Если клиента не освободить, то программа может быстро исчерпать все ресурсы.

По идее в функции `query` можно было бы обойтись простым вызовом: 
`const result = await client.query(text, params);`
но так нагляднее.

> Прим. функция `query` вызывается в цепочке middleware с глобальным перехватчиком ошибок. Поэтому при разрыве соединения с БД, событие `error`, возникающее на клиенте будет перехвачено глобально. Обработчик ощибок пула соединений при этом не сработает.

```js
const pool = new Pool({
  user: config.postgres.user,
  host: config.postgres.host,
  database: config.postgres.database,
  password: config.postgres.password,
  port: config.postgres.port,

  // idleTimeoutMillis - количество миллисекунд,
  // в течение которых клиент должен бездействовать в пуле и не быть проверенным
  // прежде чем он будет отключен от бэкенда и удален
  // по умолчанию 10000 (10 секунд) — установите 0,
  // чтобы отключить автоотключение неработающих клиентов
  idleTimeoutMillis: 10000,

  // connectionTimeoutMillis - количество миллисекунд ожидания
  // до истечения времени ожидания при подключении нового клиента
  // по умолчанию это 0, что означает отсутствие тайм-аута
  connectionTimeoutMillis: 10000,
  
  // max - максимальное количество клиентов,
  // которое должно содержаться в пуле
  // по умолчанию установлено значение 10.
  max: 50,
});

// обязательно повешать обрабочик ошибок на событие "error" для пула соединений
pool.on('error', (error) => {
  logger.error(`pool db error: ${error.message}`);
});

module.exports.query = async (text, params) => {
  const client = await pool.connect();
  const result = await client.query(text, params);
  client.release();
  return result;
};
```

#pg 