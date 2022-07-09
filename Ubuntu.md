# Ubuntu
- [Документация на русском](https://help.ubuntu.ru/wiki/%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0)
- [Пользователи в LINUX](https://techlist.top/linux-users-types-of-users/)
- [Как узнать IP-адрес Linux](https://losst.ru/kak-uznat-ip-adres-linux)

- [[Создать самоподписанный SSL сертификат]]
- [[посмотреть куда установлена программа]]
- [[VPN для Ubuntu]]
- [[sudo]]

- [Работа с пакетами apt-get](https://wiki.debian.org/AptCLI)

Поиск по командам [[Docker]]-a и их описаниям совпадения с "cont":
```
$ docker help | grep cont
```

Запуск программы в фоновом режиме:
```
$ programmName &
```
В терминал будет выведен id процесса, который можно затем завершить так:
 ```
 $ kill idProcess
 ```
 ### Узнать pid, запущенного процесса
 ```
 $ pgrep processName
 //например для ноды
 $ pgrep node
 //или
 $ pidof node
 ```
  [Подробнее про узнавание pid процессов](https://losst.ru/kak-uznat-pid-protsessa-v-linux)
  
  ### Завершить процесс
  Для зависшего процесса сначала надо узнать его pid, а затем вызвать команду `kill`. Но, не всё так просто... Если завис процесс ноды (ctrl + z), то узнав его pid, завершить командой `kill` не получиться. Для завершения надо передать сигнал на закрытие:
  ```
  $ kill pid -9
  ```
   Вывести список всех сигналов:
   ```
   $ kill -l
   ```
   [Подробнее про kill](https://pingvinus.ru/note/ps-kill-killall)