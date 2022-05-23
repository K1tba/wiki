# Менеджер пакетов npm

**npm** - инструмент для установки пакетов. Начиная с версии 5.2 в состав npm входит npx.

**npx** - интструмент для исполнения пакетов БЕЗ установки

Подробнее [о различии между npm и npx](https://www.geeksforgeeks.org/what-are-the-differences-between-npm-and-npx/#:~:text=Npm%20is%20a%20tool%20that,pollution%20in%20the%20long%20term.)

К примеру можно создать проект [[React]] и удалить все файлы и пакеты, которые требовались только для установки

```
$ npx create-react-app my-app
```

Для сравнения, чтобы использовать `create-react-app` в npm, выполните следующие команды: 
```
$ npm install create-react-app
```
, а затем 
```
$ create-react-app myApp
```
(требуется установка).