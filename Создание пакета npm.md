# Создание пакета npm
Хорошая [статья](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c) про создание [[npm]] пакета [[TypeScript]]

### Пошаговое руководство

#### 1. Создать [[GIT]] репозиторий
#### 2. [[NPM|Инициировать]] проект
#### 3. Добавить файл `.gitignore`
В файле `.gitignore`, помимо прочего целесообразно указать директорию для файлов сборки пакета. Пример файла:

```
node_modules
/lib
```

#### 4. Установить [[TypeScript]] в качестве зависимости
```bash
npm i --save-dev typescript
```

#### 5. [[Старт проекта|Создать конфигурационный]] файл [[TypeScript]]

```bash
tsc --init
```

В файле `tsconfig.json` надо добавить директивы `include` и `exclude`. В директории `outDir` указать путь каталога файлов сборки, а также установить `declaration` в `true`.

>Важно: обязательно указать `"declaration": true` для компиляции файлов `*.d.ts`. Typescript также экспортирует определения типов вместе с скомпилированным кодом javascript, поэтому пакет можно использовать как с Typescript, так и с Javascript

Файл `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "declaration": true,
    "outDir": "./lib",
    "declarationDir": "./types",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true, 
    "skipLibCheck": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "**/test/*"]
}
```


#### 6. Собрать проект
В файле `package.json` прописать скрипт сборки проекта:
```json
{
...
	script: {
		"build": "tsc"
	}
...
}
```

#### 7. Настроить линтер

Установить линтер и пакеты TypeScript:

```bash
npm install --save-dev eslint-config-airbnb-typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint
```

Создать конфигурационный файл:
```bash
npm init @eslint/config
```

В файле `.eslintrc.js` в директории `extends` добавить:
```js
"extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "airbnb-base",
        "airbnb-typescript/base"
    ],
```

В директории `parserOptions` указать:

```js
"parserOptions": {
      "project": './tsconfig.json'
    },
```

Прописать команды запуска линтера в `package.json`:

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "tsc",
    "lint": "eslint ./src",
    "lint:fix": "eslint ./src --fix"
  },
```

Настроить правила для линтера.

#### 8. Указать "белый" список файлов для публикации

Можно внести файлы/папки в черный список в файле **.npmignore**, но это не лучшая идея, т.к. придётся постоянно добавлять туда записи при появлении новых файлов.
Лучше всего указать файлы для публикации в `package.json`:

```json
{
"files": ["lib", "types"]
}
```

В публикуемый пакет будет включена только папки `lib` и `types`! ( **README.md** и **package.json** добавляются по умолчанию). Подробнее [здесь](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#files)


#### 9. Добавить тесты
Подробно про настройку [[Тестирование TypeScript|тестирования TypeScript]]


#### 10. Почитать про скрипты package.json
https://docs.npmjs.com/cli/v8/using-npm/scripts

#### 11. Некоторые замечания по файлам типов

Для того чтобы пакет можно было нормально использовать в проектах надо в файле `package.json` указать директиву с "основным" файлом и файлом с типами:
```json
{
...
	"main": "./lib/Converter",
	"types": "./types/Converter"
...
}
```

Если этого не сделать, тогда не получится импортировать пакет в проект.

Не совсем понятно как объединить файлы типов так, чтобы у них был единый файл "входа". Хорошо когда в пакете один файл типов, а если несколько?

И, чтобы файлы с типами были опубликованы надо в `package.json` добавить директорию в `files`

```json
{
...
	"files": [
	    "lib",
	    "types"
  ]
}
```

#### 12. Опубликовать пакет

>Чтобы опубликовать изменения в пакете, надо сделать коммит, вызвать `npm version patch`, а затем опубликовать

Для регистрации:
```
npm adduser
```

Для авторизации:
```
npm login
```

Публикация пакета:
```
npm publish
```

Новая версия:
```
npm version patch
```



#### Примеры конфигов

package.json
```json
{
	"name": "md-conv",
	"version": "0.0.1",
	"description": "markdown converter",
	"main": "./lib/Converter.js",
	"types": "./types/Converter"
	"scripts": {
		"test": "jest --config jest.config.js",
		"build": "tsc",
		"lint": "eslint ./src",
		"lint:fix": "eslint ./src --fix"
	},
	"repository": {
		"type": "git",
		"url": "git+https://github.com/GeoS74/md-conv.git"
	},
	"keywords": [
		"markdown",
		"html",
		"converter"
	],
	"author": "GeoS",
	"license": "ISC",
	"bugs": {
	"url": "https://github.com/GeoS74/md-conv/issues"
	},
	"homepage": "https://github.com/GeoS74/md-conv#readme",
	"devDependencies": {
		"@types/jest": "^29.0.2",
		"@typescript-eslint/eslint-plugin": "^5.37.0",
		"@typescript-eslint/parser": "^5.37.0",
		"eslint": "^8.23.1",
		"eslint-config-airbnb-typescript": "^17.0.0",
		"jest": "^29.0.3",
		"ts-jest": "^29.0.1",
		"typescript": "^4.8.3"
	},
	"files": [
	    "lib",
	    "types"
  ]
}
```


tsconfig.json
```json
{
	"compilerOptions": {
		"target": "es2020", 
		"module": "commonjs", 
		"declaration": true,
		"outDir": "./lib", 
		"declarationDir": "./types",
		"esModuleInterop": true, 
		"forceConsistentCasingInFileNames": true, 
		"strict": true, 
		"skipLibCheck": true
	},
	"include": ["src"],
	"exclude": ["node_modules", "**/test/*"]
}
```

.eslintrc.js
```js
module.exports = {
	env: {
		"browser": true,
		"es2021": true
	},
	extends: [
		"eslint:recommended",
		"plugin:@typescript-eslint/recommended",
		"airbnb-base",
		"airbnb-typescript/base"
	],
	overrides: [],
	parser: "@typescript-eslint/parser",
	parserOptions: {
		project: './tsconfig.json'
	},
	plugins: ["@typescript-eslint"],
	rules: {
		'no-param-reassign': 'off',
		'class-methods-use-this': 'off',
		'no-restricted-syntax': 'off',
		'import/prefer-default-export': 'off',
	}
}
```


jest.config.js
```js
module.exports = {
	clearMocks: true,
	coverageProvider: "v8",
	transform: {
		"^.+\\.(t|j)sx?$": "ts-jest"
	},
};
```





### Некоторые вопросы про создание [[NPM|npm]] пакета [[TypeScript]]

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