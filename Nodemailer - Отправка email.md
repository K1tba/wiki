# Nodemailer - Отправка email в [[NodeJS]]

[Документация](https://nodemailer.com/)

Для тестирования отправки email с помощью Nodemailer-a можно создать тестовый аккаунт. Получив, login и пароль можно смело логиниться на 
https://ethereal.email/

Создать тестовый аккаунт:
```
const nodemailer = require('nodemailer')

nodemailer.createTestAccount((err, account) => {
    // create reusable transporter object using the default SMTP transport

    let transporter = nodemailer.createTransport({
        host: 'smtp.ethereal.email',
        port: 587,
        secure: false, // true for 465, false for other ports
        auth: {
            user: account.user, // generated ethereal user
            pass: account.pass  // generated ethereal password
        }
    });

    console.log(account)
});
```


Пример кода:
```
    let transporter = nodemailer.createTransport({
        host: 'cargobox.site',
        port: 465,
        secure: true,
        auth: {
            user: 'noreply@cargobox.site',
            pass: 'secretpass',
        },
    })

    let result = await transporter.sendMail({
        from: '"Node js" <noreply@cargobox.site>',
        to: 'gsirotkin@yandex.ru',
        subject: 'Привет',
        text: 'This message was sent from Node js server.',
        html: 'This <i>message</i> was sent from <strong>Node js</strong> server.',
    })
```

Проверить соединение:
```
transport.verify(function (error, success) {
    if (error) {
      console.log(error);
    } else {
      console.log("Server is ready to take our messages");
    }
  });
```
