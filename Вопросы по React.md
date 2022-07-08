1. при запуске приложения где указывается вызываемый файл _/src/index.js_ и связанный с ним файл _/public/index.html_?
2. есть ли какой-нибудь способ экспортировать ]компонент по типу как в [[NodeJS]] `exports.module`? Чтобы не писать так: `import {Star} from "../Star/Star"`
а писать вот так: `import Star from "../Star/Star"`, чтобы затем использовать:
`<Star.any_component />`