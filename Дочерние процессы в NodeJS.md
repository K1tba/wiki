
# Дочерние процессы в NodeJS

Простой пример создания дочернего процесса:

index.js

```js
const bot = childProcess.fork('./child_process/bot.process', ['123456789'], {env:{test: 'foo'}})

bot.on('message', message => {
 console.log(`bot send: ${message}`)
})

bot.send('hello')
```

./child_process/bot.process

```js
class Bot {

    constructor(){
        process.on('message', message => {
            this.say(message)
        })
    }

    say(message){
        console.log(`parent send: ${message}`)
        process.send('trololo')

        console.log(process.pid)
        console.log(process.argv)
        console.log(process.env.test)
    }
}

new Bot()
```

Вывод в консоль:
```
parent send: hello
bot send: trololo
232
[
  'C:\\Program Files\\nodejs\\node.exe',
  'C:\\Users\\User\\nodejs\\bridge\\child_process\\bot.process',
  '123456789'
]
foo
```