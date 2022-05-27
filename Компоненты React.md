# Компоненты React

Компоненты бывают классовые и функциональные.

Пример классового компонента:
```
export class Menu extends React.Component { //или React.PureComponent
    render(props) {
        console.log(this.props) //доступ к props

        return <>
            <ul>
                <li>first</li>
                <li>second</li>
            </ul>
        </>
    }
}
```

Пример функционального компонента:
```
export function Menu() {
    return <>
        <ul>
            <li>first</li>
            <li>second</li>
        </ul>
    </>
}
```

В классовом компоненте метод `render()` является обязательным. Доступ к _props_ и _state_ осуществляется через `this`. В функциональном компоненте первый аргумент функции это `props`, второй `state`.

> В файлах компонентов надо импортировать React `import React from "react"` если используется версия до 18. Начиная с 18 версии React импортируется по дефолту.