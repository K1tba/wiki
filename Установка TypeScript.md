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

#tsconfig #typescriptinstall