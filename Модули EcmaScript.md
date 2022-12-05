# Модули EcmaScript

### Импорты и экспорты
[Читай подробнее про import и экспорт](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export)
[Learnjs про импорты и экспорты](https://learn.javascript.ru/import-export)

файл index.ts
```ts
import {user, users} from "./src/hello"
console.log(users);
```

файл /src/hello.ts
```ts
export const user = {name: 'Tom'}
export const users = [{name: 'Tom'}, {name: 'Jerry'}]
```

#module #import #export