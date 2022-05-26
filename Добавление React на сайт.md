# Добавление React на сайт

Можно сразу писать приложение на [[React]] и использовать 1 html-элемент в качестве root-a. А можно добавить React компоненты в уже созданное приложение.
В первом случае удобно воспользоваться вызовом:
```
npx create-react-app
```


Во-втором случае к странице добавляется загрузка внешних библиотек:
```
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>

<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
```

[пример добавления React компонента без create-react-app](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605)