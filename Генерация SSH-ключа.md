# Генерация SSH-ключа

[статья на github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

1) генерация SSH-ключа
`$ ssh-keygen -t ed25519 -C "your_email@example.com"`

2) добавление вашего SSH-ключа в ssh-agent
```
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_ed25519
```

3) копирование открытого ключа SSH в буфер обмена
[статья на github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
`$ clip < ~/.ssh/id_ed25519.pub`