## Client side rendering

![CSR](../assets/img/CSR.png)
___

## Server side rendering

![SSR](../assets/img/SSR.png)
___

## href in frames

`window.location.href` - указывает url текущей страницы; для фрейма вернёт урл фрейма.

`top.location.href` - урл самого верхнего окна, для фрейма вернёт урл самого верхнего по иерархии окна, создавшего  
фрейм; то же что `window.top.location.href`.
___

## Запретить показывать страницу в фрейме

`X-Frame-Options` - заголовок ответа сервера для управления отображением контента в фрейме; значения:  
- `DENY`
- `SAMEORIGIN`
- `ALLOW-FROM <domain>`
___


