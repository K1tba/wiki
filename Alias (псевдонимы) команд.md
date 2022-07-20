# Alias (псевдонимы) команд [[Ubuntu]]

[подробнее про псевдонимы](https://losst.ru/poleznye-alias-linux)

Для установки псевдонимов нужно откорректировать файл __.bashrc__
Поиск файла:

```
$ sudo find / -name "hrc"
```

в файле __/home/geos/.bashrc__  добавить:

```
alias foo='ls -l'
```

Теперь можно использовать так:
```
$ foo
```

Выведет список файлов в текущей директории.

Есть предложение не засорять .bashrc и прописывать alias-ы в файл /etc/profile.d/aliases.sh или можно создать рядом файл .bash_aliases и писать в него.

#ubuntu #alias