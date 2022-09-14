# Тестирование TypeScript

### JEST
Подробные инструкции по настройке тестирования [[TypeScript]] с помощью JEST:
[link 1](https://jestjs.io/ru/docs/getting-started)
[link 2](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c)

Для начала надо установить пакеты:
```bash
npm install --save-dev jest ts-jest @types/jest
```

Причём сам JEST лучше установить глобально.

Создать файл конфигурации можно командой:
```bash
jest --init
```

После создания конфигурационного файла надо прописать в него:
```json
{
	...
"transform": {
    "^.+\\.(t|j)sx?$": "ts-jest"
  },
	...
}
```

Описание этой магии [здесь](https://jestjs.io/ru/docs/next/code-transformation):
Jest выполняет код вашего проекта как JavaScript, но если вы используете синтаксис, который не поддерживается Node. js из коробки (например, JSX, типы из TypeScript, шаблоны Vue и т. д.), то сначала нужно транспилировать ваш код в чистый JavaScript, по аналогии сборки проектов для клиентской части.

В Jest это можно сделать через опцию конфигурации [`transform`](https://jestjs.io/ru/docs/next/configuration#transform-objectstring-pathtotransformer--pathtotransformer-object) .

Трансформер - это модуль, который предоставляет метод преобразования исходных файлов. Например, если вы хотите иметь возможность использовать новую языковую функцию в своих модулях или тестах, которые еще не поддерживаются Node, вы можете подключить препроцессор кода, который преобразует будущую версию JavaScript в текущую.

Пример команды запуска тестов в файле package.json:
```json
"scripts": {
    "test": "jest --config jest.config.js",
  },
```


По умолчанию команда `jest --init` создаст файл `jest.config.ts`. Как запустить тесты с конфигом `.ts` я не понимаю, поэтому пришлось переписать на `.js`


#jest #ts-jest