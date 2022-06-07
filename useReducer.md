# useReducer

Этот [[Хуки React|хук]] позволяет использовать в качестве состояния произвольный объект, для его изменения отдельно описывается функция.

Пример:

```
import styles from './styles.module.css'
import { useReducer } from "react";

const actionTypes = {
    changeName: 'changeName',
    changeText: 'changeText',
    changeRating: 'changeRating',
}

const reducer = (state, action) => {
    switch (action.type) {
        case actionTypes.changeName:
            return { ...state, name: action.payload, text: '', rating: 0 };

        case actionTypes.changeText:
            return { ...state, text: action.payload };

        case actionTypes.changeRating:
            return { ...state, rating: Number.parseInt(action.payload) || 0 };

        default:
            return state;
    }
}

export const NewReviewForm = () => {
    const [formState, dispatch] = useReducer(reducer, { name: 'Default name', text: '', rating: 0 });

    return (
        <div className={styles.root}>
            <h3>New Review</h3>
            <span className={styles.title}>Name</span>
            <input value={formState.name} onChange={(event) => {
                dispatch({ type: actionTypes.changeName, payload: event.target.value });
            }} />

            <span className={styles.title}>Text</span>
            <input value={formState.text} onChange={(event) => {
                dispatch({ type: actionTypes.changeText, payload: event.target.value });
            }} />

            <span className={styles.title}>Rating</span>
            <input value={formState.rating} onChange={event => {
                dispatch({ type: actionTypes.changeRating, payload: event.target.value })
            }} />
        </div>
    );
}
```
Здесь отдельно описывается функция `reducer`, а доступ к ней осуществляется через `dispatch`.
Причём, функция `reducer` первым аргументом получает текущий объект состояния (в данном примере это `formState`), а во второй аргумент `action` передаётся объект с котороым вызывается `dispatch`.
Функция `reducer`  должна возвращать новый объект состояния.

В принципе, в примере выше можно использовать хук [[useState]] и будет тот же эффект, но `useReducer` позволяет более гибко обрабатывать сложный объёкт состояния.

```
import styles from './styles.module.css'
import { useState } from 'react'

export const NewReviewForm = () => {
    const [state, changeState] = useState({ name: 'GeoS', text: 'any text' }

    return <div className={styles.root}>
        <h3>New Review</h3>

        <span className={styles.title}>Name</span>
        <input value={state.name} onChange={event => {
            changeState({ ...state, name: event.target.value, text: '' })
        }} />

        <span className={styles.title}>Text</span>
        <input value={state.text} onChange={event => {
            changeState({ ...state, text: event.target.value })
        }} />
    </div>
}
```
