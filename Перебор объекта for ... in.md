# Перебор объекта for ... in

В [[TypeScript]] можно перебрать в цикле все ключи объекта, а вот обратиться к значениям по этим ключам не получится. Пример:
```typescript
const obj = {
  name: 'Ann',
  age: 15
}

for(const k in obj) {
	console.log(k); //ok
	console.log(obj[k]); //ERROR
}
```

### Варианты обхода проблемы
1. Объект не имеет описанного интерфейса:
```typescript
const obj = {
  name: 'Ann',
  age: 15
}

let k: keyof typeof obj;

for(k in obj) {
  console.log(obj[k]);
}
```

2. Объект имеет описанный интерфейс, используя `keyof`:

```typescript
interface IFoo {
  name: string,
  age: number,
}

const obj: IFoo = {
  name: 'Ann',
  age: 15
}

let k: keyof IFoo;

for(k in obj) {
  console.log(obj[k]);
}
```

3. Объект имеет описанный интерфейс, без использования `keyof`:

```typescript
interface IFoo {
  name: string,
  age: number,
  [index: string]: string | number;
}

const obj: IFoo = {
  name: 'Ann',
  age: 15
}

for(const k in obj) {
  console.log(obj[k]);
}

// или упрощенный вариант интерфейса
interface IFoo {
  [index: string]: string | number;
}

const obj: IFoo = {
  name: 'Ann',
  age: 15
}

for(const k in obj) {
  console.log(obj[k]);
}
```

[Документация по интерфейсам](https://www.typescriptlang.org/docs/handbook/interfaces.html)