# Создание пакета npm
Хорошая [статья](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c) про создание [[npm]] пакета [[TypeScript]]

### Пошагово

1. Создать [[GIT]] репозиторий
2. [[менеджер пакетов npm|Инициировать]] проект
3. Добавить файл `.gitignore`
4. Установить [[TypeScript]] в качестве зависимости
```
npm i --save-dev typescript
```
5. [[Старт проекта|Создать конфигурационный]] файл [[TypeScript]]
```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "declaration": true,
    "outDir": "./lib",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true, 
    "skipLibCheck": true
  },
  "include": ["./src"],
  "exclude": ["./node_modules"]
}
```

>Важно: обязательно указать `"declaration": true` для компиляции файлов `*.d.ts`

6. Собрать проект
7. 



### Некоторые вопросы про создание [[менеджер пакетов npm|npm]] пакета [[TypeScript]]

- Куда помещать typescript и javascript файлы?  
	*Ответ: Куда вам удобно*
	
- Для чего нужно создавать папку `dist`?  
*Ответ: Обычно вы пишете исходники в TS. Потому компилируете его в JS и публикуете JS. Обычно в папке dist лежит именно скомпилированный js. *

- Какой файл указывать в `package.json`, `.ts` или `.js`?  
*Ответ: `.js`*

- Надо ли создавать `.d.ts` файлы?  
*Ответ: Да*

- Добавлять ли `"type": "module"` в `package.json`?  
*Ответ: Зависит от того, в какой формат вы компилируете ваш JS*

- Использовать `export` или `module.export`s?  
*Ответ: Зависит от того, в какой формат вы компилируете ваш JS*

- Надо ли создавать `@types/проект`, и если да что нужно делать там?
*Ответ: Нет. Это нужно только в тех случаях, если ваши d.ts файлы не включены в сам пакет*



### Почитать
- [Создание и публикация пакета npm [[TypeScript]]](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c)
- [Советы о публикации пакета npm](https://blog.npmjs.org/post/165769683050/publishing-what-you-mean-to-publish)