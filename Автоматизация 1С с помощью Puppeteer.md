# Автоматизация 1С с помощью [[Puppeteer]]
Логин и пароль не сохраняю специально
```
const puppeteer = require('puppeteer');
const path = require('path');

(async () => {
    try{
        const browser = await puppeteer.launch({
            headless:false,                     //отображать браузер
            devtools: false,                    //показать панель разработчика
            args: ['--window-size=1858,893']    //размер браузера
        });

        const page = await browser.newPage();

        await page.setViewport({                   //размер страницы
            width: 1840,
            height: 750
        });

        //step 1 - подключение к серверу
        await page.goto('https://web-1c.kamaz.ru/DS/ru_RU/');

        //pause
        await pause(5000);

        //step 2 - ввод логин и пароля
        await page.$eval('#userName', el => el.value = login);
        await page.$eval('#userPassword', el => el.value = password);
        await pause(2000);
        await page.click('#okButton');

        //pause
        await pause(12000);

        //step 3 - клик по кнопке 'Ещё'
        //BUG REPORT
        //такой вызов не работает await page.click('#curCmdCell_cmd_more');
        //попытки имитировать клик по координатам тоже не работают с кнопкой 'Ещё', хотя с другими кнопками работает

        const curCmdCell_cmd_more = await page.$('#curCmdCell_cmd_more');
        curCmdCell_cmd_more.click();

        //pause
        await pause(2000);

        //step 4 - клик по 'Номенклатура'
        //BUG DETECTED
        //в хроме кнопка 'Номенклатура' имеет идентификатор #popupItem15
        //в chromium почему-то #popupItem15
        //походу эти кнопки при отрисовке получают свои идентификаторы

        const popupItem13 = await page.waitForSelector('#popupItem13');
        popupItem13.click();

        //pause
        await pause(8000);

        //step 5 - клик по кнопке 'Отчёты'
        const frame = await page.waitForSelector("#pagesArea");
        const rect = await page.evaluate(el => {
            const {x, y} = el.getBoundingClientRect();
            return {x, y};
        }, frame);

        const offset = {x: 350, y: 57};
        await page.mouse.click(rect.x + offset.x, rect.y + offset.y);

        //pause
        await pause(2000);

        //step 6 - клик по выпадающему списку
        await page.mouse.click(rect.x + offset.x, rect.y + offset.y + 25);

        //pause
        await pause(5000);

        //step 7 - 'Сформировать отчёт'
        // это тоже работает await page.mouse.click(rect.x + 25, rect.y + offset.y + 35);

        await page.keyboard.down('ControlLeft');
        await page.keyboard.press('Enter');
        await page.keyboard.up('ControlLeft');

        //pause
        await pause(65000);

        //step 8 - 'Скачать отчёт'
        await page.keyboard.down('ControlLeft');
        await page.keyboard.press('KeyS');
        await page.keyboard.up('ControlLeft');

        //pause
        await pause(1000);

        //step 9 - 'Подтвердить отчёт'
        await page.keyboard.down('ControlLeft');
        await page.keyboard.press('Enter');
        await page.keyboard.up('ControlLeft');

        //pause
        await pause(30000);

        await browser.close();

    } catch(error){
        console.log(error.message);
    }


async function pause(delay, page){
    await new Promise(res => setTimeout(_ => res(), delay));
}
```