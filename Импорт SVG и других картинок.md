# Импорт SVG и других картинок
Импорт как [[Компоненты React|компонент React]]:
```
import {ReactComponent as Foo} from './imgs/thumb-up.svg'
...
<Foo /> //для отрисовки
```
По идее перед таким импортом надо импортировать `ReactComponent`. Только пока не понятно, что это за зверь такой `ReactComponent` и откуда его импортировать.

Если просто импортировать [[SVG]] вот так:
```
import ThumbUp from './imgs/thumb-up.svg'
```
То `ThumnUp` получит ссылку на [[SVG]]-шку и её уже можно использовать в src:
```
<img src={ThumbUp} loading="lazy"/>
```
`loading="lazy"` - это ленивая подгрузка картинок.
> Не забывай [[Оптимизация SVG|оптимизировать SVG]] перед использованием