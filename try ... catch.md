# try ... catch
### Обработка ошибок

В [[TypeScript]] достаточно проблематично обрабатывать ошибки, т.к. в JavaScript может быть "выброшена" не только ошибка, но и всё что угодно. Например:
```
throw 'string';
throw [];
throw function(){}
```

Это приводит к тому, что ошибка имеет [[Типы в TypeScript|тип]] unknown. Чтобы [[TypeScript]] смог адекватно обработать эту ситуацию, надо дополнительно проверить что за сущность "пришла" в блок `catch`:

```typescript
try {
	...
}
catch(error: unknown){

    if(error instanceof Error){
      console.log(error.message)
    }
  }
```

### Ошибки [[NodeJS]]

Для обработок ошибок [[NodeJS]] можно использовать __предикаты__. Пример:

```typescript
function isNodeError(error: unknown): error is NodeJS.ErrnoException {
  return error instanceof Error
}

try {
  ...
} catch (error) {
	
  if(isNodeError(error) && error.code === 'ENOENT'){
	//теперь можно работать с объектом ошибки NodeJS
    ...
  }
}
```

После [[Установка типов для NodeJS|добавление типов в NodeJS]] будет доступен файл: `node_modules/@types/node/globals.d.ts`, описывающий доступные типы и интерфейсы. В частности объект ошибки, который нас интересует, описан так:

```typescript
interface ErrnoException extends Error {
	errno?: number | undefined;
	code?: string | undefined;
	path?: string | undefined;
	syscall?: string | undefined;
}
```

Функция `isNodeError(...)`, описанная выше, как бы говорит: "если я верну `true`, то это точно объект с интерфейсом `NodeJS.ErrnoException` (здесь `NodeJS` это пространство имён)". 
Это поведение может приводить к неожиданным результатам.
Пример возникновения ошибки в run-time, а не в моменте компиляции:

```typescript
function isString(value: unknown): value is string {
  return true
}

const foo: unknown = [1, 2];

if(isString(foo)) {
  foo.toUpperCase() //ERROR
}
```

Читай подробнее про [[Предикаты]].

1. [решение подсмотрено здесь](https://overcoder.net/q/1122469/%D0%B2-typescript-%D0%BA%D0%B0%D0%BA-%D0%B2%D1%8B-%D0%B4%D0%B5%D0%BB%D0%B0%D0%B5%D1%82%D0%B5-%D1%80%D0%B0%D0%B7%D0%BB%D0%B8%D1%87%D0%B8%D0%B5-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-node-%D0%B8-%D0%B2%D0%B0%D0%BD%D0%B8%D0%BB%D1%8C%D0%BD%D1%8B%D0%BC%D0%B8-%D1%82%D0%B8%D0%BF%D0%B0%D0%BC%D0%B8-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA#3330283)
2. [похожее решение, но выглядит сложнее](https://dev.to/jdbar/the-problem-with-handling-node-js-errors-in-typescript-and-the-workaround-m64)

#error #ошибки #предикаты #trycatch