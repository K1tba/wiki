# Создание docker контейнера с Puppeteer

Создать [[Image - образы Docker|образ docker]], который на борту имеет [[Puppeteer]] оказалось не самой простой задачей. Попытка решить её в лоб не увенчалась успехом. Вот это создаст образ и даже запустит [[Контейнеры Docker|контейнер]], но при попытке вызова [[Puppeteer]] контейнер вылетит с ошибкой:
Dockerfile:
```
FROM node
WORKDIR /pssr
COPY package.json .
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3250
CMD ["node", "./dist/index"]
```

Документация по [использованию puppeteer в docker](https://pptr.dev/guides/docker) однозначного решения не содержит, взамен предлагается в качестве основы использовать их `Dockerfile` для сборки образа. Сам `Dockerfile` от авторов puppeteer [лежит здесь](https://github.com/puppeteer/puppeteer/blob/main/docker/Dockerfile). Выглядит он так:

```
FROM node:18@sha256:cd7fa8f136023f7500490e410ba70dd3982ccca21805264f2a260a3a97be7376

# Install latest chrome dev package and fonts to support major charsets (Chinese, Japanese, Arabic, Hebrew, Thai and a few others)
# Note: this installs the necessary libs to make the bundled version of Chrome that Puppeteer
# installs, work.
RUN apt-get update \
    && apt-get install -y wget gnupg \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/googlechrome-linux-keyring.gpg \
    && sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/googlechrome-linux-keyring.gpg] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-khmeros fonts-kacst fonts-freefont-ttf libxss1 dbus dbus-x11 \
      --no-install-recommends \
    && service dbus start \
    && rm -rf /var/lib/apt/lists/* \
    && groupadd -r pptruser && useradd -rm -g pptruser -G audio,video pptruser

USER pptruser

WORKDIR /home/pptruser

COPY puppeteer-browsers-latest.tgz puppeteer-latest.tgz puppeteer-core-latest.tgz ./

ENV DBUS_SESSION_BUS_ADDRESS autolaunch:

# Install @puppeteer/browsers, puppeteer and puppeteer-core into /home/pptruser/node_modules.
RUN npm i ./puppeteer-browsers-latest.tgz ./puppeteer-core-latest.tgz ./puppeteer-latest.tgz \
    && rm ./puppeteer-browsers-latest.tgz ./puppeteer-core-latest.tgz ./puppeteer-latest.tgz \
    && (node -e "require('child_process').execSync(require('puppeteer').executablePath() + ' --credits', {stdio: 'inherit'})" > THIRD_PARTY_NOTICES)

CMD ["google-chrome-stable"]
```

Куча строк разных инструкций... ну ок. Начинаем разбираться. Первым делом бросается в глаза то, что используется определённый образ [[NodeJS]], где-то в документации сказано, что версия хрома и ноды должны быть совместимы. Хорошо, запомним.

Дальше идёт установка  Chrom-a, причём в последней строке  `&& groupadd -r pptruser && useradd -rm -g pptruser -G audio,video pptruser`  внутри [[Image - образы Docker|образа]] создаётся пользователь `pptuser`. Зачем, мне не известно. Далее идёт ещё одна интересная инструкция:
`COPY puppeteer-browsers-latest.tgz puppeteer-latest.tgz puppeteer-core-latest.tgz ./`
Здесь копируются в образ некие архивы. И тут начинается интересное, если попробовать собрать образ на основе этих инструкций, сборка прервётся с ошибкой, т.к. у нас нет этих архивов. Ок, смотрим в [репозитории дальше](https://github.com/puppeteer/puppeteer/blob/main/docker/pack.sh) и обнаруживаем некий [[bash скрипты|bash скрипт]] с именем `pack.sh`. Выглядит он так:

```sh
#!/bin/bash

# Packs puppeteer using npm and moves the archive file to docker/puppeteer-latest.tgz.
# Expected cwd: project root directory.

set -e

cd docker

npm pack --workspace puppeteer --workspace puppeteer-core --workspace @puppeteer/browsers --pack-destination .

rm -f puppeteer-core-latest.tgz
rm -f puppeteer-latest.tgz
rm -f puppeteer-browsers-latest.tgz

mv puppeteer-core-*.tgz puppeteer-core-latest.tgz
mv puppeteer-browsers-*.tgz puppeteer-browsers-latest.tgz
mv puppeteer-[0-9]*.tgz puppeteer-latest.tgz
```

Просто взять его и запустить тоже не получится, сначала он будет ругаться на отсутствие пространств имён `--workspace`. Разбираться с этим не очень хотелось, поэтому я поубирал эти пространства имён, убрал дальнейшие инструкции по удалению и перемещению файлов и выполнив скрипт получил заветные архивы.
Казалось бы проблема сборки образа решена и действительно, строка `COPY puppeteer-browsers-latest.tgz puppeteer-latest.tgz puppeteer-core-latest.tgz ./` выполнилась, но процесс упал на следующей инструкции `Dockerfile`

```
RUN npm i ./puppeteer-browsers-latest.tgz ./puppeteer-core-latest.tgz ./puppeteer-latest.tgz \
    && rm ./puppeteer-browsers-latest.tgz ./puppeteer-core-latest.tgz ./puppeteer-latest.tgz \
    && (node -e "require('child_process').execSync(require('puppeteer').executablePath() + ' --credits', {stdio: 'inherit'})" > THIRD_PARTY_NOTICES)
```

Тут надо отметить ещё одну граблю, после получения архивов, их имена содержали точную версию, а не `latest`. Т.е. выглядели примерно так:  `puppeteer-browsers-1.7.21.tgz`
Прописав в `Dockerfile` правильные имена архивов и на всякий случай раздав им [[chmod|права на выполнение]] попытка сборки образа опять потерпела фиаско...

### Решение
После этого пришло понимание, что я иду куда-то не туда. Переосмыслив происходящее, я понял что при сборке образа, единственное что надо было сделать это не полагаться на установку [[Puppeteer]] с помощью [[NPM]], а надо было установить Chrome вручную, поэтому в свой `Dockerfile` я добавил инструкцию по установке хрома и теперь он выглядел так:
Dockerfile:
```
FROM node:18@sha256:cd7fa8f136023f7500490e410ba70dd3982ccca21805264f2a260a3a97be7376

WORKDIR /pssr

RUN apt-get update \
&& apt-get install -y wget gnupg \
&& wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/googlechrome-linux-keyring.gpg \
&& sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/googlechrome-linux-keyring.gpg] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
&& apt-get update \
&& apt-get install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-khmeros fonts-kacst fonts-freefont-ttf libxss1 dbus dbus-x11 \
--no-install-recommends \
&& service dbus start \
&& rm -rf /var/lib/apt/lists/*

COPY package.json .
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3250
CMD ["node", "./dist/index"]
```

Образ собрался, но при вызове Puppeteer, контейнер вылетал с ошибкой `Error: Failed to launch chrome!` - браузер не найден! Решение было подсмотрено [здесь](https://pptr.dev/troubleshooting#running-puppeteer-on-gitlabci), правда оно касалось не совсем моей проблемы, но всё же сработало. Надо было в код запуска [[Puppeteer]] добавить аргументы в конфигурацию, а именно запускать надо так:

```ts
const browser = await puppeteer.launch({
	args: ['--no-sandbox', '--disable-setuid-sandbox']
});
```

Теперь всё работает. )

#puppeteer #docker