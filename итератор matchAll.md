# итератор matchAll

При использовании `matchAll(...)` со строками, возвращается итератор. Использование на чистом JavaScript выглядит так:

```javascript
const foo = 'example string'.matchAll(/\w+/);
console.log([...foo])
```


Этот код на [[TypeScript]] упадёт с ошибкой:
```typescript
const foo: IterableIterator<RegExpMatchArray> = 'example string'.matchAll(/\w+/);

const matched = [...foo];
```

Чтобы это исправить надо в файл `tsconfig.js` добавить директиву `target` и указать версию `es2015` или выше:
```json
{
  "compilerOptions": {
	...
    "target": "es2015",
	...
  },
}
```

Если надо компилировать в `es5`, то необходимо включить `downLevelIteration`:

```json
{
  "compilerOptions": {
    "target": "es5",
    "downlevelIteration": true
  }
}
```