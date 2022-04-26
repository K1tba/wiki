# Использование https в Koa
```
const https = require('https');
const Koa = require('koa');
const app = new Koa();
const fs = require('fs')
const options = {
	key: fs.readFileSync('key.pem'),
	cert: fs.readFileSync('cert.pem')
}

https.createServer(options, app.callback()).listen(3000, _ => {
	console.log('server run https://localhost:', 3000);
});
```

[[Создать самоподписанный SSL сертификат]]