# Инициализация БД с передачей аргументов Docker compose

При запуске скриптов инициализации базы данных, иногда надо передать параметры конфигурации в скрипты. Если прописать код скриптов инициализации в фале `.sql`, то не понятно как можно прокинуть переменные. Чтобы это сделать можно использовать bash  скрипты. Пример передачи переменной в  [[bash скрипты|bash скрипт]]  инициализации БД:
```yml
version: '1.0'
services:
	app:
		build: .
		volumes:
			- initdb:/mauth/libs/
		environment:
			- VERIFICATION_TTL="10 minute"
	db:
		image: "postgres"
		volumes:
		- initdb:/docker-entrypoint-initdb.d
volumes:
	initdb:
```

Тогда в скриптах инициализации БД [[PostgreSQL]] можно получить переменную `VERIFICATION_TTL` так:
```bash
updatedat < NOW() - INTERVAL ${VERIFICATION_TTL};
```