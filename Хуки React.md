# Хуки React
В [[React]] используются функциональные и классовые [[Компоненты React|компоненты]]. Для передачи состояния в классовых компонентах проблем нет, т.к. это по сути объект в котором можно легко хранить в свойствах какие-то значения. С функциональными компонентами сложнее. В них не так просто передавать переменные между вызовами, поэтому используются Хуки.

Пример передачи состояния при клике по кнопке:
```
export const Product = ({name}) => {
	let [productCount, setProductCount] = useState(0);

	const down = () => {
		setProductCount(productCount - 1);
	};

	const up = () => {
		setProductCount(productCount + 1);
	};
	
	return (<div className={styles.root}>
		<span>{name}</span>
		<div className={styles.actions}>
			<button onClick={down} className={styles.action}>
				<img src={ThumbDown} className={styles.icon} loading="lazy"/>
			</button>
			<span className={styles.count}>{productCount}</span>
			<button onClick={up} className={styles.action}>
				<img src={ThumbUp} className={styles.icon} loading="lazy"/>
			</button>
		</div>
	</div>)
}
```

##### Стандартные хуки [[React]]
1. [[useState]]
2. [[useReducer]]