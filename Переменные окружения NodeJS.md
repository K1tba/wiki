# Переменные окружения NodeJS

Установка переменных окружения при запуске [[NodeJS|NodeJS приложения]] для разных систем. После запуска приложения переменные доступны внутри приложения через `process.env`

### Linux

```bash
NAME=foo node index
```

### Windows

Это работает на _Windows Server 2016 Standart_
```powershell
$env:NAME="production"
node index
```




#process #переменныеокружения