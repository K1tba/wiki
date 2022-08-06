# Telegram bot с помощью Telegraf

[Проект Telegraf на GitHub](https://github.com/telegraf/telegraf)

Пример простого бота:

```
const { Telegraf } = require('telegraf');
const bot = new Telegraf(config.bot.token);

//ответ бота на команду /start
bot.start((ctx) => ctx.reply(`Привет, человек!!!`));

//ответ бота на команду /help
bot.help((ctx) => ctx.reply('Send me a sticker'));

bot.on('sticker', (ctx, next) => {...});
bot.on('text', (ctx, next) => {...});

//обработчик конкретного текста
bot.hears('hello world', (ctx, next) => {...});

// запуск бота
bot.launch();
```

#bot #telegraf #telegrambot