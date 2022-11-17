# Запуск остановка сервера mongod

Для начала нужно [[Узнать систему инициализации|узнать систему инициализации]] операционной системы. Затем в зависимости от результата использовать или `systemctl` или `service`.

##### Для systemct

Запустить mongod:

```bash
$ sudo systemctl start mongod
```

Если вы получаете сообщение об ошибке, похожее на следующее, при запуске [`mongod`](https://www.mongodb.com/docs/manual/reference/program/mongod/#mongodb-binary-bin.mongod) `Failed to start mongod.service: Unit mongod.service not found.`

Сначала выполните следующую команду:

```bash
$ sudo systemctl daemon-reload
```

Остановить mongod:

```bash
$ sudo systemctl stop mongod
```

Перезапустить mongod:

```bash
$ sudo systemctl restart mongod
```

Посмотреть состояние:
```bash
$ sudo systemctl status mongod
```

Убедиться, что MongoDB запустится после перезагрузки системы, введя следующую команду:

```bash
$ sudo systemctl enable mongod
```


#mongod