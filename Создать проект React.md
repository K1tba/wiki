# Создать проект [[React]]

Создать react проект с помощью [[менеджер пакетов npm | npx]]:
```
$ npx create-react-app my-app
```
npx поставляется с [[менеджер пакетов npm | npm]] начиная с версии 5.2+

Создать react проект с помощью npm initializer (доступен начиная с npm 6+):
```
npm init react-app my-app
```

По сути эти 2 способа идентичны


Можно создать проект с помощью npm с установкой временных пакетов:
```
$ npm install create-react-app
$ create-react-app myApp
```
этот вариант я не проверял, т.к. не понятно куда после первой команды установятся npm-пакеты

[Подробнее про начало работы с React](https://create-react-app.dev/docs/getting-started)