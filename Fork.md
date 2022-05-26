# FORK

после создания форка его надо скачать
`git clone ...`

дальше зайти в основной репозиторий и скопировав ссылку выполнить
`git remote add upstream <ссылка на репозиторий>`

проверить:
```
git remote -v
```
(origin - черновик(fork), upstream - основной репозиторий)

```
origin  git@github.com:student/nodejs-20190329_student.git (fetch)
origin  git@github.com:student/nodejs-20190329_student.git (push)
    upstream  git@github.com:js-tasks-ru/nodejs-20190329_student.git (fetch)
    upstream  git@github.com:js-tasks-ru/nodejs-20190329_student.git (push)
```

получить обновления основного репозитория
`git pull upstream master`


создать коммит
`git commit -am "add solution for 1-module 1-task"`

отправить свои изменения в основной репозиторий
`git push origin master`

зайти на страницу fork-a и нажать create pull request