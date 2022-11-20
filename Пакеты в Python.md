# Пакеты в Python

[Установка пакетов](https://packaging.python.org/en/latest/tutorials/installing-packages/)
[Руководство pip](https://pip.pypa.io/en/latest/user_guide/)
[Установка pip](https://packaging.python.org/en/latest/guides/installing-using-linux-tools/#debian-ubuntu)
[Что такое wheels](https://realpython.com/python-wheels/)

Пакеты в  [[Python]] устанавливаютсяс помощью пакетного менеджера **pip**.
По умолчанию пакеты устанавливаются глобально, но есть возможность устанавливать их локально. Для этого надо создать [[Виртуальная среда Python|виртуальную среду]] и сделать её активной.

### pip

Установка
```bash
sudo apt update
sudo apt install python3-venv python3-pip
```

Проверть версию:
```bash
python3 -m pip --version
// или коротко
pip --version
```

Установка пакета:
```bash
pip install SomePackage
```

Удаление пакета:
```bash
pip uninstall SomePackage
```

Список пакетов:
```bash
pip list
```

>Поиск пакетов помощью `pip search` не работает. Для поиска надо использовать вебинтерфейс.


### Что ещё умеет делать `pip`:

-   `pip install package_name` - установка пакета(ов).
-   `pip download package_name` - загружает пакет(ы), но не устанавливает.
-   `pip uninstall package_name` - удаление пакета(ов).
-   `pip list` - выводит список установленных пакетов.
-   `pip freeze` - выводит список установленных пакетов с их версиями для файла requirements.txt.
-   `pip search` - поиск пакетов в PyPI по их имени.
-   `pip install -U` - обновление пакета(ов).
-   `pip show some-package-name` - показывает информацию об установленном пакете.
-   `pip check package_name` - проверяет что установленные пакеты имеют совместимые зависимости.
-   `pip install --force-reinstall` - переустановить пакет, даже если он последней версии.
-   `pip help` - помощь по доступным командам.

### Зависимости

Есть возможность указать список необходимых пакетов для установки в файле. [Подробнее здесь](https://pip.pypa.io/en/latest/user_guide/#requirements-files)

Пример установки:
```bash
python3 -m pip install -r requirements.txt
```

Для фиксации установленных пакетов и дальнейшей их установки:
```bash
python3 -m pip freeze > requirements.txt
python3 -m pip install -r requirements.txt
```



#pip