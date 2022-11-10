# GIT

Почитать:
[Неплохая статья про GIT](https://techrocks.ru/2022/02/02/git-github-learning-games/)
[Заметки про GIT и не только...](https://github.com/rsajob/docs/wiki/#git)
[online тренажёр](https://learngitbranching.js.org/?locale=ru_RU)
[Полезные команды если надо что-то откатить](https://tproger.ru/translations/problems-with-git/)


1) создание нового репозитория
```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:GeoS74/weed.git
git push -u origin main # -u - привязать локальную ветку к удалённой
```

[зачем нужен upstream в GIT](https://stackoverflow.com/questions/17122245/what-is-a-git-upstream)

```
git push -u origin main
```
это короткая форма:
```
git branch --set-upstream-to my_branch origin/my_branch
git push
```

Вообще, при работе с Github команда `git push origin main` работает одинаково как с флагом `-u`, так и без него.

2) получение репозитория
`git clone https://github.com/GeoS74/weed.git direct` (здесь direct это какая-то папка, которая будет создана если её нет)

### [[Генерация SSH-ключа]]

### [[Fork]]

### [[Branch - ветки в GIT]]

### [[GIT cheat sheet]]

