# PM2
[сайт проекта с документацией](https://pm2.keymetrics.io)

 Менеджер запуска процессов [[NodeJS]]
 
### Установка
```
$ npm i -g pm2
```
### Конфигурационный файл pm2
В корне проекта можно создать файл с конфигурацией запуска pm2 в форматах:
- js
- json
- yaml
Конфигурационный файл можно создать вручную и назвать как угодно, например: `pm2.config.js`.

##### Автоматическое создание файла конфигурации
Все команды ниже, создадут файл `ecosystem.config.js`.

это создаст файл `ecosystem.config.js` в корне проекта с дефолтным кодом.
```
$ pm2 ecosystem

// эквивалентно
$ pm2 init
```

Это создаст минималистичный конфиг.
```
$pm2 init simple
```


Примеры:
###### формат js
```
module.exports = {
	apps : [{
		script: 'index.js',
		name: 'jungle book'
	}]
};
```

###### формат json
```
{
	"apps": [{
		"name": "boo",
		"script": "index.js"
	}]
}
```
###### формат yaml
```
apps:
	- name: chat-app
	  script: index.js
```

### Запуск приложения
##### запуск в единственном экземпляре
Если используется конфигурационный файл (допустим  `pm2.config.js` ), то запустить приложение можно так:
```
$ pm2 start pm2.config.js
```

##### запуск нескольких экземпляров
Для этого в конфигурационном файле нужно дописать:

```
module.exports = {
	apps : [{
		script: 'index.js',
		name: 'jungle book',
		instances: 3
	}]
};
```
[Подробнее про директивы конфигурационного файла](https://pm2.keymetrics.io/docs/usage/application-declaration/)

### Перезапуск приложения
 Для перезапуска есть 2-е команды:

```
$ pm2 restart app_name
$ pm2 reload app_name
```

 В принципе они делают одно и то же. Но, `reload` вроде как не должен допускать downtime приложения. При релоаде создаётся несколько новых экземпляров, затем не сколько старых останавливаются и процесс повторяется пока не будут запущены только новые инстансы.
 
 При использовании `restart` возможен downtime.
[reload vs restart pm2](https://pm2.keymetrics.io/docs/usage/cluster-mode/#reload)

### Основные команды
```
$ pm2 start app_name
$ pm2 restart app_name
$ pm2 reload app_name
$ pm2 stop app_name
$ pm2 delete app_name

$ pm2 kill
$ pm2 [list|ls|l|status]


$ pm2 log app_name
$ pm2 logs
```

### Режимы запуска
Приложение может быть запущено в режиме `cluster` или `fork`(по умолчанию).
Чтобы явно указать режим надо в конфигурационном файле прописать `exec_mode`:
```
module.exports = {
	apps : [{
		script: 'index.js',
		name: 'jungle book',
		max_memory_restart: '2G',
		instances: 3,
		exec_mode: 'cluster'
	}]
};
```

### Ограничение памяти
Можно установить ограничение на использование памяти для инстанса. Для этого в конфигурационном фале надо прописать  свойство `max_memory_restart`.
Пример:
```
module.exports = {
	apps : [{
		script: 'index.js',
		name: 'jungle book',
		max_memory_restart: '2G',
		instances: 3
	}]
};
```
В этом случае, если какой-то инстанс потребует больше 2 гигов памяти, то он будет перезапущен. Это "не явная" подстраховка на случай утечки памяти.

### Файлы логов
В конфиге можно прописать пути для файлов логов:
```
module.exports = {
	apps : [{
		script: 'index.js',
		name: 'jungle book',
		max_memory_restart: '2G',
		instances: 3,
		error: '/var/log/...',
		output: '/var/log/...',
		log: '/var/log/...'
	}]
};

```

#pm2 #config-pm2