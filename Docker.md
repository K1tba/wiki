# Docker

[Документация](https://docs.docker.com/engine/install/ubuntu/)
[Видео про Docker](https://www.youtube.com/watch?v=n9uCgUzfeRQ)
[Почитать про Docker](https://losst.ru/zapusk-kontejnera-docker)
[Приложение NodeJS + Mongo](https://docs.docker.com/language/nodejs/develop/)

Читай это: https://earthly.dev/blog/postgres-docker/
И вот это: https://habr.com/ru/post/578744/

Посмотреть установленную версию:
```bash
sudo docker version
```

В Docker-e если вывести подсказку `docker --help`, то можно заметить, что есть команды, а есть команды управления. В принципе, команды - это по сути наиболее часто используемые команды из команд управления, только в более сокращенной версии. Пример, получение списка контейнеров:

```bash
// команда управления
sudo docker container ls -a

// просто команда
sudo docker ps -a
```

### [[Установка Docker]]

### [[Image - образы Docker]]
### [[файл dockerignore]]
### [[файл Makefile]]
### [[Контейнеры Docker]]
### [[Запуск кастомного контейнера]]
### [[Передача данных при запуске контейнера]]
### [[Сети Docker контейнеров]]
### [[Volume тома хранения данных]]
### [[Флаги команд]]
### [[Использование официальных образов]]
### [[Публикация образа в Docker hub]]
### [[Установить ограничение памяти для контейнера]]

#docker