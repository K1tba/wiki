# Про чанки и highWaterMark
Немного про стримы в [[NodeJS]]

Кириллица в кодировке utf-8 имеет 2 байта, латиница - 1 байт. Пример ниже показывает как бьётся кодировка если не предусмотреть эту ситуацию заранее.
На входе есть файл `text.txt`, в который записаны 20 букв "ф":

```
фффффффффффффффффффф
```

Скрипт пытается считать эти буквы, но highWaterMark говорит, что нужно читать по 9 байт...
```
const fs = require('fs');

const stream = fs.createReadStream('text.txt', {
	highWaterMark: 9,
	// encoding: 'utf-8'
});
// stream.setEncoding('utf-8')

const data = [];
stream.on('data', chunk => {
	data.push(chunk);
	// console.log(chunk.toString('utf-8'))
});

stream.on('end', () => {
	console.log(Buffer.concat(data).toString('utf-8'));
})
```

#chank #highwatermark