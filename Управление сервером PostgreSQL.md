# Управление сервером PostgreSQL

В [[Ubuntu]] демон сервера [[PostgreSQL]] называется `postgresql`. Для управления сервером можно использовать следующие команды:
```bash
// проверка состояния
sudo systemctl status postgresql

// остановка 
sudo systemctl stop postgresql

// запуск
sudo systemctl start postgresql

// перезапуск
sudo systemctl restart postgresql
```