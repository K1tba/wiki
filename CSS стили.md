# CSS стили
##### Импорт глобальных стилей
```
import "./styles.css"
```
При подключении стилей "глобально" имена классов не модифицируются под [[Компоненты React]]


##### Импорт стилей как объект
```
import styles from "./styles.module.css"
```

>Стили пишутся в файл примерно с таким именем: `file_name.module.css`
При этом обязательн онадо указывать `module.css` в названии файла, иначе при импорте стилей объект со стилями будет пустой.

##### Использование нескольких стилей
Если надо указать несколько классов на одном компоненте, то надо писать примерно следующее:
```
<div className={`${styles.root} ${styles.act} foo`}>
</div>
```
Это не очень удобно и можно заменить на использование пакета [classnames](https://www.npmjs.com/package/classnames). Тогда можно просто перечислять CSS классы через запятую и если их нет они будут добавлены:
```
import styles from './styles.module.css';
import classnames from "classnames";

const Menu = ({menu, className}) => (
    <div className={classnames(styles.root, className)}>
    </div>
);
```