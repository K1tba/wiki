# Варианты для Ubuntu

1. torsocks может не работать из-за того, что [не открыт порт](https://www.binarytides.com/proxify-applications-with-torsocks/) на котором Tor будет прослушивать локальные подключения от [[Tor]] (9051)
/etc/tor/torrc
```
SocksPort 9050
ControlPort 9051
```

2. попробовать поставить утилиту [Nyx](https://manpages.ubuntu.com/manpages/impish/man1/nyx.1.html) для мониторинга Tor из репозиториев [[Ubuntu]]

3. Debian не использует репозитории Tor. Попробовать [[Список репозиториев|закомментировать репозитории]] в Ubuntu и установить [[Tor]]

4. Проверить все ли пакеты установлены ориентируясь на [[Решение для Debian|зависимости tor в Debian]]. Для вывода [[Узнать список, установленных пакетов|списка установленных пакетов]].

5. Посмотреть [[Решение для Debian|конфигурацию torsocks]] в файле /etc/tot/torsocks.conf

6. Посмотреть логи
- /var/log/tor
 