# Эхо сервер NodeJS

Пример простого эхо-сервера на [[NodeJS]] с использованием [[Transform стримы|transform стрима]]:

```
const stream = require('stream');
const http = require('http');

class Logger extends stream.Transform {
  constructor(options){
    super(options)
  }

  _transform(chunk, _, callback){
    console.log(chunk.toString());
    callback(null, chunk);
  }
}

const server = http.createServer();

server.on('request', (req, res) => {
  res.statusCode = 200
  req.pipe(new Logger()).pipe(res)
});

server.listen(3500);
```