# Компоненты React
#### Примеры компонентов [[React]]
Компоненты бывают классовые и функциональные.

Пример классового компонента:
```js
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
```js
export function Menu() {
    return <>
        <ul>
            <li>first</li>
            <li>second</li>
        </ul>
    </>
}
```

[[Пустой элемент в React|Читай про пустой элемент в React]]

В классовом компоненте метод `render()` является обязательным. Доступ к _props_ и _state_ осуществляется через `this`. В функциональном компоненте первый аргумент функции это `props`, второй `state`.

> В файлах компонентов надо импортировать React `import React from "react"` если используется версия до 18. Начиная с 18 версии React импортируется по дефолту.

Внутри функции  рендеринга можно указывать условную логику отрисовки. Главное правило: должно что-то возвращаться. Пример:
```js
export const Restaurants = ({restaurants}) => {
	return <div>
		{1 ? <Restaurant restaurant={restaurants[0]} /> : true}
	</div>
}
```
При этом вот так не сработает:
```js
export const Restaurants = ({restaurants}) => {
	return <div>
		{
			if(1) <Restaurant restaurant={restaurants[0]} />
			else <Restaurant restaurant={restaurants[0]} />
		}
	</div>
}
```
Условная отрисовка позволяет использовать определённые простейшие логические операции. Конструкции типа `for(...)` также не работают внутри фигурных скобок.
>NOTE 
>описать здесь React.CreateElement(...)
#### Рендеринг компоненты
 Перерендеринг происходит при изменении пропсов, состояния или force-update
#### Отрисовка нескольких компонентов
Внутри фигурных скобок можно указывать несколько сомпонентов для отрисовки, в этом случае компоненты должны быть переданы массивом:
```js
export const Restaurants = ({restaurants}) => {
	return <div>
		{
			[
				<Restaurant />,
				<Restaurant />
			]
		}
	</div>
}
```
Можно перебрать массив, полученный из пропсов и отрисовать компоненты:
```js
export const Restaurants = ({restaurants}) => {
	return <div>
		{restaurants.map((rest) => <Restaurant restaurant={rest}/>)}
	</div>
}
```
> Такой подход сработает, только в консоли будет warning:
`Warning: Each child in a list should have a unique "key" prop.` Перев.: `Предупреждение: каждый дочерний элемент в списке должен иметь уникальный ключевой реквизит.`

Чтобы исправить эту ошибку и убрать warning надо добавить атрибут __key__:
```js
export const Restaurants = ({restaurants}) => {
	return <div>
		{restaurants.map((rest, index) => <Restaurant key={index} restaurant={rest}/>)}
	</div>
}
```

#### Уникальные ключи компоненты
При отрисовке массива компонентов, чтобы [[React]] адекватно мог монтировать новые не затрагивая старые компоненты, нужно передать уникальный ключ. Если ключ будет неуникальным - будет ещё один warning:
>`Warning: Encountered two children with the same key, `1`. Keys should be unique so that components maintain their identity across updates. Non-unique keys may cause children to be duplicated and/or omitted — the behavior is unsupported and could change in a future version.` Перев.: `Обнаружены два дочерних элемента с одним и тем же ключом «1». Ключи должны быть уникальными, чтобы компоненты сохраняли свою идентичность при обновлениях. Неуникальные ключи могут привести к дублированию и/или пропуску дочерних элементов — такое поведение не поддерживается и может измениться в будущей версии.`

Пример передачи ключа:
```js
export const Restaurants = ({restaurants}) => {
	return <div>
		{restaurants.map((rest) => <Restaurant key={rest.id} restaurant={rest}/>)}
	</div>
}
```


При последующих отрисовках React будет сравнивать компоненты по этому ключу и монтировать только новые компоненты, оставляя остальные (если они не обновляются) без изменений.
Обновление ключа у существующего компонента приведет к перемонтированию всего компонента.

#### [[Импорт SVG и других картинок|Импорт SVG]] как компонент
```js
import {ReactComponent as Foo} from './imgs/thumb-up.svg'
...
<Foo /> //для отрисовки
```
По идее перед таким [[Импорт SVG и других картинок|импортом SVG]] надо импортировать `ReactComponent`. Только пока не понятно, что это за зверь такой `ReactComponent` и откуда его импортировать.
