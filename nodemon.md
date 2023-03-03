
# nodemon

### Проблема запуска в Windows

После глобальной установки, запуск nodemon в Windows 11 падал с ошибкой:

```
nodemon : Невозможно загрузить файл C:\Program Files\nodejs\nodemon.ps1, так как выполнение сценариев отключено в этой системе. Для получения дополнительны
х сведений см. about_Execution_Policies по адресу https:/go.microsoft.com/fwlink/?LinkID=135170.
строка:1 знак:1
+ nodemon index
+ ~~~~~~~
    + CategoryInfo          : Ошибка безопасности: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```


Для устранения проблемы надо запустить терминал от админа и выполнить:
```
Set-ExecutionPolicy RemoteSigned
```

Решение подсмотрено [здесь](https://ru.stackoverflow.com/questions/935212/powershell-%D0%B2%D1%8B%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81%D1%86%D0%B5%D0%BD%D0%B0%D1%80%D0%B8%D0%B5%D0%B2-%D0%BE%D1%82%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%BE-%D0%B2-%D1%8D%D1%82%D0%BE%D0%B9-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B5)


#nodemon