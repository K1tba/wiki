# Варианты для Ubuntu

Установка Tor:
```
$ sudo apt install tor
```

(был установлен Tor версии 0.4.7.8)
Работает без privoxy.

##### Проверка

```
$ tor --version
$ netstat -ltupan | grep 905
$ wget -qO - https://api.ipify.org; echo
$ torsocks wget -qO - https://api.ipify.org; echo
```

##### Конфигурация
 /etc/tor/torrc
```
SocksPort 9050
SocksPort 9052

ClientTransportPlugin obfs4 exec /usr/bin/obfs4proxy 
Bridge 82.193.158.28:9002 1FC8B9B2E92E5004B729AE34DEEC213A628824DA
Bridge 77.96.91.103:443 ED000A75B79A58F1D83A4D1675C2A9395B71BE8E

UseBridges 1 
```

/etc/tor/torsocks.conf

```
TorAddress 127.0.0.1
TorPort 9050
OnionAddrRange 127.42.42.0/24
```

##### Cписок [[Узнать список, установленных пакетов|установленных пакетов]]

deb.torproject.org-keyring/now 2022.04.27.1 all [установлен, локальный]
gir1.2-appindicator3-0.1/focal,now 12.10.1+20.04.20200408.1-0ubuntu1 amd64 [установлен]
gnome-calculator/focal,now 1:3.36.0-1ubuntu1 amd64 [установлен, автоматически]
gnome-shell-extension-appindicator/focal-updates,focal-updates,now 33.1-0ubuntu0.20.04.2 all [установлен]
gnome-system-monitor/focal-updates,now 3.36.1-0ubuntu0.20.04.1 amd64 [установлен, автоматически]
language-selector-common/focal-updates,focal-updates,now 0.204.2 all [установлен, автоматически]
language-selector-gnome/focal-updates,focal-updates,now 0.204.2 all [установлен, автоматически]
libappindicator1/focal,now 12.10.1+20.04.20200408.1-0ubuntu1 amd64 [установлен, автоматически]
libappindicator3-1/focal,now 12.10.1+20.04.20200408.1-0ubuntu1 amd64 [установлен, автоматически]
libatk-adaptor/focal-updates,now 2.34.2-0ubuntu2~20.04.1 amd64 [установлен, автоматически]
libgirepository-1.0-1/focal-updates,now 1.64.1-1~ubuntu20.04.1 amd64 [установлен, автоматически]
libqt5waylandcompositor5/focal,now 5.12.8-0ubuntu1 amd64 [установлен, автоматически]
libraptor2-0/focal-updates,focal-security,now 2.0.15-0ubuntu1.20.04.1 amd64 [установлен, автоматически]
php-phpmyadmin-motranslator/focal,focal,now 5.0.0-1 all [установлен, автоматически]
python3-secretstorage/focal,focal,now 2.3.1-2ubuntu1 all [установлен, автоматически]
tor-geoipdb/now 0.4.7.8-1~focal+1 all [установлен, локальный]
tor/now 0.4.7.8-1~focal+1 amd64 [установлен, локальный]
torsocks/focal,now 2.3.0-2 amd64 [установлен]
usb-creator-common/focal,now 0.3.7 amd64 [установлен, автоматически]
usb-creator-gtk/focal,now 0.3.7 amd64 [установлен, автоматически]


##### Trouble shooting
1. Пакет privoxy в файле конфигурации __/etc/bash.bashrc__ требует указания следующих строк:
```
export all_proxy="socks://localhost:9050/"
export http_proxy="http://localhost:8118/"
export https_proxy="http://localhost:8118/"
export no_proxy="localhost,127.0.0.1,::1,192.168.1.1,192.168.0.1"
```

После удаления пакета __privoxy__ из-за этих настроек упал [[NodeJS]], npm пакеты перестали устанавливаться. Т.к. пакет privoxy был удалён, то в системе не было процесса слушающего порт 8118, а трафик шёл как раз через этот порт.



2. попробовать поставить утилиту [Nyx](https://manpages.ubuntu.com/manpages/impish/man1/nyx.1.html) для мониторинга Tor из репозиториев [[Ubuntu]]

3.  Логи /var/log/tor

#tor
 