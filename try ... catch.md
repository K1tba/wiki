# try ... catch

В JavaScript может быть "выброшена" не только ошибка, но и всё что угодно. Например:
```
throw 'string';
throw [];
throw function(){}
```

Поэтому в блоке `catch` не возможно сказать заранее, что туда прилетит. Чтобы [[TypeScript]] смог адекватно обработать эту ситуацию, надо дополнительно проверить что за сущность "пришла" в блок `catch`:

```
try {
	...
}
catch(error: unknown){

    if(error instanceof Error){
	
      console.log(error.message)
    }
  }
```