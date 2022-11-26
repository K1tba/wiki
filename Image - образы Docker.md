# Image - образы Docker

Хранилище образов  [[Docker]]-a: https://hub.docker.com

На основе образов запускаются [[Контейнеры Docker]]

Посмотреть список установленных образов:
```bash
sudo docker image ls

//или
sudo docker images
```


### Получение готового образа

Получить образ:
```bash
sudo docker pull <image_name>
```

Все образы, полученные из [[Docker]] hub, а также созданные вручную сохраняются в каталог: `/var/lib/docker/overlay2`

### Создание своего образа

В каталоге с проектом надо создать файл: `Dockerfile`, а в нём прописать инструкции по созданию образа. Пример:
```dockerfile
FROM node

WORKDIR /myapp

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "index"]
```

После создания __Dockerfile__ можно создать кастомный образа запустив в консоли:
```bash
sudo docker build .
```

Если код приложения не подвергался изменениям, то повторный запуск этой команды не создаёт дубликаты образов. В противном случае будет создан новый образ.

#### Улучшенный вариант создания образа

```dockerfile
FROM node

WORKDIR /myapp

COPY package.json .
# вариант с явным указанием рабочей директории
# COPY package.json /myapp

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "index"]
```

Если создавать [[Image - образы Docker|образы]] с таким __Dokerfile__, то вывод в консоль будет примерно таким.
###### Создание нового образа в новой рабочей директории:
```bash
sudo docker build .
Sending build context to Docker daemon  1.341MB
Step 1/7 : FROM node
 ---> c71adfc6ec58
Step 2/7 : WORKDIR /testdocker
 ---> Running in 5791f0fc1397
Removing intermediate container 5791f0fc1397
 ---> f6321b414d70
Step 3/7 : COPY package.json /testdocker
 ---> c2e4f7cb247a
Step 4/7 : RUN npm install
 ---> Running in d7f655cc62d8

added 47 packages, and audited 48 packages in 9s

found 0 vulnerabilities
Removing intermediate container d7f655cc62d8
 ---> 8d0d9639e22d
Step 5/7 : COPY . .
 ---> 4a37b3fa8d17
Step 6/7 : EXPOSE 3000
 ---> Running in 95a8896b997b
Removing intermediate container 95a8896b997b
 ---> 517a90ae901c
Step 7/7 : CMD ["node", "index"]
 ---> Running in 79661b8c0712
Removing intermediate container 79661b8c0712
 ---> c41c24f2509d
Successfully built c41c24f2509d
```

###### Повторное создание образа
```bash
sudo docker build .
Sending build context to Docker daemon  1.341MB
Step 1/7 : FROM node
 ---> c71adfc6ec58
Step 2/7 : WORKDIR /testdocker
 ---> Using cache
 ---> f6321b414d70
Step 3/7 : COPY package.json /testdocker
 ---> Using cache
 ---> c2e4f7cb247a
Step 4/7 : RUN npm install
 ---> Using cache
 ---> 8d0d9639e22d
Step 5/7 : COPY . .
 ---> 3fbdda7e5361
Step 6/7 : EXPOSE 3000
 ---> Running in 4534199c6117
Removing intermediate container 4534199c6117
 ---> 65db1b221b7a
Step 7/7 : CMD ["node", "index"]
 ---> Running in 7e83425ea447
Removing intermediate container 7e83425ea447
 ---> 58d3b7814140
Successfully built 58d3b7814140

```

###### Некоторые пояснения вывода в консоль
При повторном создании образа с улучшенной конфигурацией файла __Dockerfile__ некторые шаги создания образа не выполняются, а берутся из кеша. Например шаг `Step 3/7` не копирует заново файл `package.json`, а берёт его из кеша. То же самое, происходит и с шагом `4/7`, [[NPM|npm-пакеты]] не устанавливаются по новой, т.к. файл `package.json` не изменялся, то и пакеты устанавливать бессмысленно, они берутся из кеша.

Шаг `Step 2/7` также кешируется - если рабочая директория существует она не пересоздаётся.


### Сборка кастомного образа


```bash
sudo docker build -t foo:bar .
```


### Инспектирование образа
```bash
sudo docker inspect <image_name>
```