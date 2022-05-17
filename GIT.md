# GIT

Почитать:
[Неплохая статья про GIT](https://techrocks.ru/2022/02/02/git-github-learning-games/)


1) генерация ssh ключа
[статья на github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
`$ ssh-keygen -t ed25519 -C "your_email@example.com"`

2) добавление вашего SSH-ключа в ssh-agent
```
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_ed25519
```

3) копирование открытого ключа SSH в буфер обмена
[статья на github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
`$ clip < ~/.ssh/id_ed25519.pub`

4) создание нового репозитория
```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:GeoS74/weed.git
git push -u origin main
```

5) получение репозитория
`git clone https://github.com/GeoS74/weed.git direct` (здесь direct это какая-то папка, которая будет создана если её нет)

### FORK

после создания форка его надо скачать
`git clone ...`

дальше зайти в основной репозиторий и скопировав ссылку выполнить
`git remote add upstream <ссылка на репозиторий>`

проверить:
`git remote -v`
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

### [[Branch - ветки в GIT]]