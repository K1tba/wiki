# eslint with TypeScript
[[Линтер]] для [[TypeScript]] требует своих пакетов и плагинов.

1. Установить линтер и пакеты для TypeScript:
```
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint
```

>Не используй `tslint`, т.к. этот пакет устарел

Эти пакеты будут проверять код [[TypeScript]] на валидность и не будут заниматься форматированием. См. п.3 Установка пакетов для форматирования

2. Создать конфинурационный файл `.eslintrc.cjs` (см. ниже)

>Если проект не использует ESModule, то файл можно назвать  `.eslintrc.js`

```js
module.exports = {
  extends: [
	  'eslint:recommended', 
	  'plugin:@typescript-eslint/recommended',
  ],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  root: true,
};
```


3. Установить [пакеты](https://www.npmjs.com/package/eslint-config-airbnb-typescript) для форматирования кода [[TypeScript]]

```bash
npm install eslint-config-airbnb-typescript --save-dev
```



4. Использовать [[Линтер|линтер]]
```
npx eslint .
```


Пример файла конфигурации `eslintrc.json` для проекта [[TypeScript]]:

```json
{
	extends: [
		'eslint:recommended',
		'plugin:@typescript-eslint/recommended',
		'airbnb-base',
		'airbnb-typescript',
	],
	parser: '@typescript-eslint/parser',
	plugins: ['@typescript-eslint'],
	root: true,
	ignorePatterns: ['*.js'],
	parserOptions: {
		project: './tsconfig.json'
	},
	rules: {
		'react/jsx-filename-extension': 'off',
		'no-param-reassign': 'off',
		'class-methods-use-this': 'off',
		'no-restricted-syntax': 'off',
		'import/prefer-default-export': 'off',
	}
}
```

>Если в файле tsconfig указана некая директория, которая не будет включаться в проект при компиляции (exclude), то при попытки линтинга файлов такой директории будет ошибка. Для её исправления нужно в файле .eslintrc указать, что такую директорию надо игнорировать (ignorePatterns)


[Eslint with TypeScript](https://typescript-eslint.io/docs/)
[Подробнее про eslintrc](https://eslint.org/docs/latest/user-guide/configuring/configuration-files)
[eslint и tslint](https://khalilstemmler.com/blogs/typescript/eslint-for-typescript/)

#linter #линтер #eslint