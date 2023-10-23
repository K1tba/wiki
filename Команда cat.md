# Команда cat

Эта команда расшифровывается как "Compact and Align Text" :-)

[Читать описание команды cat](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/cat.html)

Вывести список всего, что связано с `_cat`:
```bash
curl "localhost:9200/_cat/?pretty"
```


##### Получить список индексов
[Подробнее про cat indices](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/cat-indices.html)

Вывести список [[Index API|всех индексов]] можно так:

```bash
curl -X GET "http://localhost:9200/_cat/indices?v&pretty"
```

### Вставить текст в файл с помощью cat

Классный вариант (сработал внутри [[Контейнеры Docker|контейнера docker]] где не было   `nano`):
```bash
$ cat > myfyle.txt <<EOF
lorem
ipsum
EOF
```

Ещё один не плохой вариант:

```bash
$ cat >> myfile.txt
lorem ipsum
^D
```

где ^D - CTRL+D

[[Вставить текст в файл Ubuntu]]

#cat