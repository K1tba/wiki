# файл Makefile

Прикольная штука в  [[Docker]], позволяет вынести длинные команды по запуску [[Запуск кастомного контейнера|контейнеров]] в отдельный файл и запускать их в более короткой нотации.
В корне проекта создать файл `Makefile`, пример:

```Makefile
run:
	docker run --rm --network mynetwork -d -p 3002:3002 --name app -e DB_HOST=db -e POSTGRES_PASSWORD=admin app
stop:
	docker stop -t 0 app
rundb:
	docker run --rm --network mynetwork -d -p 5435:5432 --name db -e POSTGRES_PASSWORD=admin postgres
stopdb:
	docker stop -t 0 db
```

Теперь можно запускать и останавливать контейнер так:
```bash
sudo make run

sudo make stop
```

#makefile