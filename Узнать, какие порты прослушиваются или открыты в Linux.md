# Узнать, какие порты прослушиваются или открыты в [[Ubuntu]]

### с помощью [[netstat]]

```
$ sudo apt install net-tools
$ sudo netstat -ltupan
```

Отфильтровать вывод (к примеру для [[Tor]]):
```
$ sudo netstat -ltupan | grep /tor
```

### Посмотреть какой порт соответствует какой службе
Это информация в файле: /etc/services
```
$ less /etc/services
```

### Закрыть или открыть порт
```
sudo ufw deny 8080
sudo ufw allow 8080
```
[Читать про закрытие и открытие портов](https://itsecforu.ru/2019/11/08/%F0%9F%90%A7-%D0%BA%D0%B0%D0%BA-%D0%BD%D0%B0%D0%B9%D1%82%D0%B8-%D0%B8-%D0%B7%D0%B0%D0%BA%D1%80%D1%8B%D1%82%D1%8C-%D0%BE%D1%82%D0%BA%D1%80%D1%8B%D1%82%D1%8B%D0%B9-%D0%BF%D0%BE%D1%80%D1%82-%D0%B2-linux/)