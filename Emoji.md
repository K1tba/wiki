# Emoji

[Разбор эмодзи Unicode в JavaScript](http://crocodillon.com/blog/parsing-emoji-unicode-in-javascript)

Если пользователь [[Telegram Bot|Telegram]] отправляет не стикер, а эмодзи, то [[NodeJS|нода]] получит просто текст. Чтобы понять что этот текст являеся эмодзи можно проверить это так:

```
const ranges = [
  '\ud83c[\udf00-\udfff]', // U+1F300 to U+1F3FF
  '\ud83d[\udc00-\ude4f]', // U+1F400 to U+1F64F
  '\ud83d[\ude80-\udeff]'  // U+1F680 to U+1F6FF
];

new RegExp(ranges.join('|'), 'g').test(message)
```

#emodji #эмодзи