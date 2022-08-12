# Transform стримы

Transform стримы в [[NodeJS]] предназначены для чтения потока, каких-либо промежуточных преобразований чанков и передачи их дальше.

### Создание стрима
Для создания transform стрима в [[NodeJS]] нужно создать класс, который наследует от `stream.Transform` и реализовать метод `_transform()`:

```
const stream = require('stream');

class Logger extends stream.Transform {
  constructor(options){
    super(options);
  }

  _transform(chunk, encoding, callback){
    console.log(chunk);

    callback(null, chunk)
  }
}
```

Метод `_transform()` получает 3 аргумента:
- chunk - собственно сам чанк;
- encoding - здесь можно указать кодировку или Buffer (странная хрень). Обычно этот аргумент не используется.
- callback - функция обратного вызова callback(error, chunk)

Можно запроцессить что-нибудь с чанком и передать его дальше, в этом случае можно использовать `this.push()`. В этом случае при вызове callback функции можно не передавать чанк, а вызвать её вообще без аргументов. Пример:
```
const stream = require('stream');

class Logger extends stream.Transform {
  constructor(options){
    super(options);
  }

  _transform(chunk, encoding, callback){
	this.push(chunk)
	this.push(chunk)
    callback()
  }
}
```

### Простой пример

```
const stream = require('stream');
const fs = require('fs');

class Logger extends stream.Transform {
  constructor(options){
    super(options);
  }

  _transform(chunk, encoding, callback){
    console.log(chunk);
    callback(null, chunk)
  }
}

const reader = fs.createReadStream('lorem.txt', {highWaterMark: 9});
const logger = new Logger();

reader
  .on('error', error => {
    console.log(error.message)
    reader.destroy();
    logger.destroy();
  })
  .pipe(logger)
  .on('error', error => {
    console.log(error.message)
    reader.destroy();
    logger.destroy();
  })
  ```
  
  Тот же пример, только с использованием `pipeline`:
  
  ```
  stream.pipeline(
  	reader,
  	logger,
  	(error) => {
    	if(error){
      	console.log(error)
      	return;
    	}
    	console.log('done')
  	}
)
  ```
  
  >Смотри также пример [[Эхо сервер NodeJS]]
  
### Разница между pipe и pipeline
  
При использовании `pipeline` нельзя сказать точно в каком из стримов произошла ошибка, можно только примерно понять исходя из описания ошибки. Это из-за того, что `pipeline` принимает последним аргументом функцию обработки ошибки, которая будет вызвана при возникновении ошибки в любом из [[Стримы|стримов]]. 
  
При использовании `pipe` можно гораздо гибче обрабатывать ошибки, т.к. обработчик ошибки "вешается" на каждый стрим в отдельности (см. пример выше).
  
С другой стороны при использовании `pipe` необходимо заботиться о закрытии стримов при возникновении ошибки, .т.к. сам факт её возникновения не приведёт к автоматическому закрытию стримов. В этом плане `pipeline` удобнее, т.к. в случае ошибки стримы будут автоматически закрыты.
>__Attention:__ При использовании `pipe`, в случае ошибки, стримы надо закрывать вручную (смю пример выше)
  
### Объектный режим стримов
  
 [[Стримы]] могут работать не только с байтами, но и с произвольными структурами данных.
 Для этого есть объектный режим. 
  >Объектный режим надо включить: 
  >`const logger = new Logger({objectMode: true});`
  
Пример:

```
const obj = [
  {
    user: {
      name: 'Geos',
      rank: 'admin',
    }
  },
  {
    music: {
      genre: 'house',
      quality: '256 kbps'
    }
  },
]

const stream = require('stream');

class Logger extends stream.Transform {
  constructor(options){
    super(options);
  }

  _transform(chunk, encoding, callback){
    this.push(chunk.time = Date.now());
    console.log(chunk);
    callback()
  }
}

const reader = stream.Readable.from(obj);
const logger = new Logger({objectMode: true});

stream.pipeline(
  reader,
  logger,
  (error) => {
    if(error){
      console.log(error)
      return;
    }
    console.log('done')
  }
)
```

Вывод в консоль:
```
{ user: { name: 'Geos', rank: 'admin' }, time: 1660296631447 }
{ music: { genre: 'house', quality: '256 kbps' }, time: 1660296631451 }
done
```

В этом примере стрим считывает объект, а трансформ стрим добавляет каждому считанному объекту новое свойство.
>Readable стрим создаётся вызовом `stream.Readable.from(obj)`

### Метод  flush()
Метод  вызывается когда стрим полностью завершил работу и используется для:
- очистки каких-нибудь данных
- записи чего-нибудь (напр. строки) в конец данных стрима

```
class Logger extends stream.Transform {
  constructor(options){
    super(options);
    this.remainder = '';
  }

  _transform(chunk, encoding, callback){
    this.remainder += 'la la la';
    callback(null, chunk)
  }

  _flush(callback){
    //запись строки после всех данных стрима
    callback(null, this.remainder);
  }
}
```


#transformstream #трансформстрим #stream #стримы