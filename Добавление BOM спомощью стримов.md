# Добавление BOM спомощью стримов

В кейсе по [[Универсальный калькулятор|созданию универсального калькулятора]] при скачивании справочника городов [[Деловые линии]] возникла необходимость добавить к .csv файлу BOM. Для этого надо было добавить `\ufeff` к эго содержимому. Решение через [[Стримы|стримы]]:

```
const ws = fs.createWriteStream(path.join(__dirname, `../files/${fname}`));

await new Promise(res => {
  ws.write("\ufeff", _ => res());
});

await new Promise(res => {
  response.body.pipe(ws);
  ws.on('close', _ => res());
});
```