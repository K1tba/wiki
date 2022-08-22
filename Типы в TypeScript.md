# Типы в TypeScript

##### Примитивные типы в [[TypeScript]]

```typescript
//Boolean
const isTrue: boolean = true;

//Number
const num: number = 2
const float: number = 12.5;
const binary: number = 0b01;

//BigInt
const bigNumber: bigint = 10n;

//Null
const nullVar: null = null;

//Undefined
const undefinedVar: undefined = undefined;
```

##### Object
```typescript
//Object
const obj: {
	name: string,
	age: number
} = {
	name: 'Tom',
	age: 15
}

//можно не указывать поля и тогда это будет объект с любой структурой
const obj1: {} = {
	name: 'Tom',
	age: 15
}
```

##### Array
```typescript
//Array
const arr: string[] = ['rank', 'date']
const arr1: {
	name: string,
	age: number
}[] = [obj, obj];
```

##### Any
```typescript
//Any
let anyVar: any = 1;
anyVar = 'string'
```

##### Unknown
```typescript
//Unknow
//требует проверки существования каких либо полей перед использованием
const unknownVar: unknown = {name: 'Tom'}
if(typeof unknownVar === 'object'){
	if(unknownVar !== null){
		if(unknownVar.hasOwnProperty('name')){
			console.log(unknownVar.name) 
			//попытка обращения к unknownVar.name вызовет ошибку, т.к. 
			//нет проверки на существование свойства 'name'
		}
	}
}
```

##### Void
```typescript
//Void
function foo(): void {}

function bar(): void {
	return undefined;
}
```

##### Литеральные типы
```typescript
//Literal Type
const liter: 400 | 500 = 400
const liter1: 500 = 500
```

#####  Tupple (кортежи)
```typescript
//Tupple (кортеж)
const tupple: [number, string] = [150, 'text']
```

##### Never
Never - значение, которого никогда не будет
```typescript
//Never
function neverFunc(): never {
	while(true){}
}

function neverFunc1(): (never {
	throw new Error()
}

function neverFunc2(): (never {
	neverFunc2()
}
```

##### Enum
###### Простые enum-ы
>После компиляции сильно "разбухает" код

```typescript
enum FontWeight {
	Normal = 99,
	Bold,
	Semibold
}

const fontWeight = FontWeight.Bold;
console.log(fontWeight === 100); // true
```

Значения полей перечисления (enum-a) `Normal`, `Bold` и `Semibold` будут соответственно 99, 100, 101.

Этот код компилируется в :
```javascript
var FontWeight;

(function (FontWeight) {
	FontWeight[FontWeight["Normal"] = 99] = "Normal";
	FontWeight[FontWeight["Bold"] = 100] = "Bold";
	FontWeight[FontWeight["Semibold"] = 101] = "Semibold";
})(FontWeight || (FontWeight = {}));

const fontWeight = FontWeight.Bold;
console.log(fontWeight === 100);
```

Если указать не числовое значение, то тогда придётся у всех полей указывать значения:
```typescript
enum FontWeight {
	Normal = '99',
	Bold = 'bold font',
	Semibold = 'semibold font'
}
```


Упрощённый пример:
```javascript
const bar = {};
bar[bar['hello'] = 5] = 'world';

console.log(bar) // { '5': 'world', hello: 5 }
console.log(bar.hello) // 5
console.log(bar[5]) // 'world'
```

###### Константные enum-ы
```typescript
const enum FontWeight {
	Normal,
	Bold,
	Semibold
}

const fontWeight = FontWeight.Bold;
```

Скомпилируется в:
```javascript
const fontWeight = 1 /* Bold */;
```

>Константные enum-ы имеют проблемы при использовании с Babel

##### Конструкция "as const" - альтернатива enum-ам
`as const` похоже на `Object.freeze()`,  существует эта конструкция только в [[TypeScript]].
Поля объекта `{...} as const` помечаются как `readonly`  и не могут быть изменены.

```typescript
const fontWeight = {
	Normal: 400,
	Bold: 500,
	Semibold: 600
} as const;

console.log(fontWeight.Normal)
```

Скомпилируется в:
```javascript
const fontWeight = {
	Normal: 400,
	Bold: 500,
	Semibold: 600
};

console.log(fontWeight.Normal); // 400
```




#типыtypescript
