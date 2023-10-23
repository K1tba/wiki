# Вставить текст в файл Ubuntu

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

Ещё больше классных вариантов [здесь](https://linux-notes.org/vstavit-tekst-v-fajl-v-unix-linux/)

