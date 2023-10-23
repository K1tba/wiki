
# react-helmet управление head

Есть хорошая библиотека [react-helmet](https://www.npmjs.com/package/react-helmet, позволяющая "на лету" изменять title страницы и meta теги в  head.

```ts
const defaultTitle = 'default title';

const defaultDescription = 'default description';

export default function Head({title, description}: Props) {

return <Helmet>
	<title>{title || defaultTitle}</title>
	<meta name="description" content={description || defaultDescription} />
</Helmet>
}
```

Есть небольшая проблема, связанная с изменением `drscription`. Код выше отработает не корректно, он просто не будет менять `description`, почему это так - не знаю, но на github в комментариях [нашлось решение](https://github.com/nfl/react-helmet/issues/341#issuecomment-1637155385):

```ts
const defaultTitle = 'default title';

const defaultDescription = 'default description';

export default function Head({title, description}: Props) {

return <Helmet onChangeClientState={() => {
	const metaDescription = document.querySelector('meta[name="description"]');
		if (metaDescription) {
			metaDescription.setAttribute('content', description || defaultDescription);
		}}
}>

	<title>{title || defaultTitle}</title>
</Helmet>
}
```



#react-helmet