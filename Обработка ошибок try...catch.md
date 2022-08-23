# Обработка ошибок try...catch
В [[TypeScript]] достаточно проблематично обрабатывать ошибки в блоке catch, т.к. ошибка имеет [[Типы в TypeScript|тип]] unknown. Из-за этого, перед обращениям к каким-либо свойствам `error` необходимо убедиться, что они есть в этом типе.

Вот пример обработки ошибок [[NodeJS]] (предварительно необходимо установить `@types/node`):

```typescript
try {
  ...
} catch (error) {
	
  if(isNodeError(error) && error.code === 'ENOENT'){
    ...
  }
}
	
const isNodeError = (error: Error): error is NodeJS.ErrnoException => {
  return error instanceof Error
}	

//Или в более читабельном виде
function isNodeError(error: unknown): error is NodeJS.ErrnoException {
  return error instanceof Error
}
```

Если я правильно понимаю, то возвращаемое значение `error is NodeJS.ErrnoException` проверяется на соответствие интерфейса `ErrnoException`. Который в свою очередь описан в файле: `node_modules/@types/node/globals.d.ts`

>Оператор `is` это "предикат"

1. [решение подсмотрено здесь](https://overcoder.net/q/1122469/%D0%B2-typescript-%D0%BA%D0%B0%D0%BA-%D0%B2%D1%8B-%D0%B4%D0%B5%D0%BB%D0%B0%D0%B5%D1%82%D0%B5-%D1%80%D0%B0%D0%B7%D0%BB%D0%B8%D1%87%D0%B8%D0%B5-%D0%BC%D0%B5%D0%B6%D0%B4%D1%83-node-%D0%B8-%D0%B2%D0%B0%D0%BD%D0%B8%D0%BB%D1%8C%D0%BD%D1%8B%D0%BC%D0%B8-%D1%82%D0%B8%D0%BF%D0%B0%D0%BC%D0%B8-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA#3330283)
2. [похожее решение, но выглядит сложнее](https://dev.to/jdbar/the-problem-with-handling-node-js-errors-in-typescript-and-the-workaround-m64)
3. [пдробнее про предикаты](https://fettblog.eu/typescript-type-predicates/)

#error #ошибки #предикаты