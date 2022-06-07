#HighOrderComponent (HOC)

По сути High Order Component (компонент высшего порядка) - это компонент обёртка для вынесения общего функционала для создания компонентов на уровень выше.

Пример:
```
export function withHoc(WrappedComponent) {
    const WrapperComponent = ({ date, ...props }) => {
        if (date > new Date()) {
            return <span></span>
        }

        return <WrappedComponent {...props} date={date}/>;
    }

    WrapperComponent.displayName = `Wrapper${WrappedComponent.displayName || WrappedComponent.name || 'Component'}`;

    return WrapperComponent;
}

const TodayDateComponent = withHoc(DateComponent);
const TodayEnFormatDateComponent  = withHoc(EnFormatDateComponent);
```

В этом примере надо обратить внимание на `WrapperComponent.displayName`, значение этого свойства должно отображаться в DevTools. (Проверить это)