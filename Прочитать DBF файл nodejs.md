# Прочитать DBF файл nodejs

Пример чтения .dbf файла [[КЛАДР]]:

```js
const path = require('path');
const {DBFFile} = require('dbffile');

async function iterativeRead() {

	let dbf = await DBFFile.open(path.join(__dirname, './KLADR.DBF'), {
	encoding:'cp866'
	});
	
	console.log(`DBF file contains ${dbf.recordCount} records.`);
	console.log(`Field names: ${dbf.fields.map(f => f.name).join(', ')}`);
	for await (const record of dbf) console.log(record);
}

iterativeRead();
```

> Здесь указывается кодировка `cp866` - это кодировка базы данных [[КЛАДР]]

#dbf #dbffile