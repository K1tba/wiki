# Установка [[TypeScript]]

##### [[Ubuntu]]

- [start project](https://nodejs.dev/learn/nodejs-with-typescript)

Установка [[TypeScript]] не глобально:
```
npm i -D typescript
```

После установки пакетов вызов компилятора упал с ошибкой:
```
geos@geos-MacBookAir:~/nodejs/typescript$ tsc

Команда «tsc» не найдена, но может быть установлена с помощью:

sudo apt install node-typescript
```

После установки пакета `node-typescript` всё работает как надо.

##### tsconfig.json

```
{
	"compilerOptions": {
		"module": "CommonJS",
		"noImplicitAny": true,
		"removeComments": true,
		"preserveConstEnums": true,
		"sourceMap": true
	},
	"include": ["."]
}


```

[Читать про компиляцию кода TypeScript](https://docs.microsoft.com/ru-ru/visualstudio/javascript/compile-typescript-code-npm?view=vs-2022)

```
{
  "compilerOptions": {
    "noImplicitAny": false,
    "noEmitOnError": true,
    "removeComments": false,
    "sourceMap": true,
    "target": "es5",
    "outDir": "dist"
  },
  "include": [
    "scripts/**/*"
  ]
}
```


#tsconfig #typescriptinstall