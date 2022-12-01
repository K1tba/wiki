# Публикация образа в Docker hub

Кастомный [[Image - образы Docker|образ]] можно опубликовать в [Docker hub](https://hub.docker.com). Для этого:
1. Создать репозиторий на __Docker hub__ (напр. `geos74/static-server`)
2. Собрать образ (имя образа должно соответствовать названию репозитория) 
3. Залогиниться, выполнив: `docker login -u <user_name> -p <user_pass>`
4. Запушить образ на [Docker hub](https://hub.docker.com)

#dockerhub