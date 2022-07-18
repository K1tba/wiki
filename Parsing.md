# Parsing

### Запустить [[Puppeteer]] через Tor


https://tor.stackexchange.com/questions/14409/shows-connection-timed-out-on-tor


1. В конфиге Tor, который в Windows лежит здесь: 
```
\Tor Browser\Browser\TorBrowser\Data\Tor
```
Откорректировать файл: `torrc`, записав в него:
```
SocksPort 9050
SocksPort 9052
SocksPort 9053
SocksPort 9054
и т.д.
```
>Необходимо пропускать порт 9051, его Tor использует под свои нужды

2. Перезапустить [[Tor]]
В [[Ubuntu]] после внесения изменений в файл torrc надо их применить перезапустив службу:
```
$ sudo /etc/init.d/tor restart
$ sudo /etc/init.d/privoxy restart # Эту команду я не проверял
```
3. Дальше [[Puppeteer]] можно запускать так:
```
const browser = await puppeteer.launch({
	headless: true, //hide browser
	args: ['--proxy-server=socks5://127.0.0.1:9050']
});
```

[Статья про этот подход](https://habr.com/ru/company/ruvds/blog/486688/)