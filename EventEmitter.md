# EventEmitter

За события в [[NodeJS]] отвечает класс EventEmitter. Все классы в NodeJS наследуют от EventEmitter.

Пример создания класса и использования кастомных событий:
```
const EventEmitter = require('events');

const ee = new EventEmitter();

ee.on('testing', str => console.log(str));

ee.emit('testing', 'hello world')
```

Пример перехвата всех событий для объекта server:
```
const http = require('http');

const server = http.createServer();

//перехват события
const emit = server.emit;
server.emit = function(event){
  console.log(event) //вывод в консоль названия события
  emit.apply(this, arguments)
}

server.on('request', (req, res) => {
  req.pipe(res)
});

server.listen(3500);
```

#ee #eventemitter