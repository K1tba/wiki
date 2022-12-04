# Конфигурация JVM в контейнере Elasticsearch

[Использование Elasticsearch](https://losst.pro/ispolzovanie-elasticsearch)

При использовании официального образа [[Elasticsearch]] файл конфигурации __JVM__ находится в директории: `/usr/share/elasticsearch/config/jvm.options`.

При использовании [[Elasticsearch]]  без [[Docker]], при установке в [[Ubuntu]] файл конфигурации __JVM__ скорее всего будет находиться здесь: `/etc/elasticsearch/jvm.options`.

>В любом случае можно попробовать найти файл конфигурации java-машины с помощью [[find|команды find]]

#jvm