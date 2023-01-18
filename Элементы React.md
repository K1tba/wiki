# Элементы [[React]]

### Создание элементов

```js
//index.js

import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
    React.createElement('div', {
        children: [
            React.createElement('h1', {children: 'Hello'}),
            React.createElement('h2', {children: 'world'}),
        ]
    })
);
```

Это эквивалентно следующему коду с использованием [[Расширения файлов .js vs .jsx|.jsx]]

```js
//index.js

import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
    <div>
        <span>I am span</span>
        <div>
            <h1>Hello</h1>
            <h2>world</h2>
        </div>
    </div>
);
```