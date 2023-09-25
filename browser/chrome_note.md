## Hot key

`CTRL+T` - new tab.

`CTRL+W` - close tab.

`CTRL+TAB` - switch between tabs.

`F5` - reload page.

`F10` - settings.

`F12` - Chrome DevTools.

`F11` - full screen.

`CTRL+H` - history tab.

`CTRL+SHIFT+TAB` - last closed tab.

`CTRL+F5/SHIFT+F5/CTRL+SHIFT+R` - hard reload.

`CTRL+D` - bookmarks tab.

`CTRL+J` - downloads.

`SHIFT+ESC` - chrome task manager.

## DevTools

`CTRL+SHIFT+P` - devtools command line.

___

## Игнорирование ошибок сертификата

- создать отдельный ярлык браузера
- в свойствах -> Target -> дописать в конце после пробела --ignore-certificate-errors

___

## Core Web Vitals Performance

> **TTFB** - time to first byte; время до получения первого байта контента клиентом.

> **FCP** - first contentful paint; время до того как браузер отрисовал первую часть контента после навигации.

> **LCP** - largest contentful paint; время до загрузки и рендера основного контента.

> **TTI** - time to interactive; время до того как пользователь может взаимодействовать со страницей.

> **CLS** - cumulative layout shift; measures visual stability to avoid unexpected layout shift (например лаги при загрузке картинок карусели).

> **FIP** - first input delay; time from when the user interacts with the page to the time when the event handlers are able to run (задержка обработки взаимодействий пользователя).

___

`{}` - в devTools/Sources в левом нижнем углу есть такая кнопка, форматирует в читаемый вид min-файлы.

___

`chrome://about` - доступ к апи из адресной строки.

___

## Браузерные движки. Layout engine

> **WebKit** - Safari, Chrome (до 2013).

> **Blink** - Chromium, Chrome (from 28 version), Edge (from 79 version), Opera (from 15), Yandex Browser.  
> Создан на основе WebKit.

> **Gecko** - Firefox. Mozilla opensource engine. 

> Браузерный движок преобразует html/css код в интерактивное изображение на экране.  
> Предоставляет свойства и методы для работы с веб-страницей.

___

## Chromium

> **Chromium** - opensource браузер, разработанный Google в коллаборации с Opera, Yandex, Nvidia, Microsoft.  
> На его основе сделан Chrome. Chromium - opensource, в отличии от Chrome.  
> Chromium можно рассматривать как альфа-версию Chrome, используется для горячего внедрения новых фич.

> С 2013 Chromium, Chrome, Chrome OS перешли на движок Blink, который по сути форк WebKit от Apple.

___

## Full height screenshot

- `CTRL+SHIFT+P` - in DevTools
- `screenshot` - ввести в поиск

___

`SHIFT+ENTER` - перенос строки в консоли.

___

## Local Storage

> Локальное хранилище браузера.  
> Для каждого домена отдельное.  
> Размер 5-10 mb.  
> Данные хранятся в парах key-value.

> Методы:
>   - `getItem()`
>   - `setItem()`
>   - `clear()`
>   - `removeItem()`

```
// example
localStorage.getItem('key');

localStorage.someKey;

localStorage.setItem('key', 'value');
```

___

## WebAssembly

> Браузерная технология, позволяет запускать в веб-приложении код, написанный на разных языках. Имеет почти нативную скорость.

> **webAssembly** - низкоуровневый ассемблерный язык, имеет читабильный текстовый формат. Код на wasm будет соблюдать политики безопасности браузера.

Использование wasm:
- портирование приложения на С/С++
- код на уровне сборки
- приложение на Rust

> **Emscripten** - компилятор С/С++ в wasm; компилирует код в файл .wasm для запуска модуля в html документе.

> Wasm-код не может напрямую обращаться к web-api, только через js-код (в будущем планируется).

___

## Ништяки DevTools

**повтор запроса** - ПКМ по запросу -> Replay XHR.

**повтор запроса с новыми параметрами** - ПКМ по запросу -> Copy as fetch -> вставить в консоли -> меняем параметры -> enter.

**копирование как объект** - ПКМ -> copy object.

**копирование html-элемента** - ПКМ -> copy element.

**копирование стилей элемента** - ПКМ -> copy styles.

**скриншот всего сайта:** - открыть консоль -> `CTRL+SHIFT+P` -> `capture full size screenshot`.

**скриншот элемента** - выделить нужный элемент в Elements -> `CTRL+SHIFT+P` -> `capture node screenshot`.

**развернуть все ноды в Elements** - `ALT+click`.

___

## CORS

> **CORS** - Cross Origin Resource Sharing; политика запрещает междоменные запросы.

Браузер различает **два вида** запросов:
- простые  
- сложные  

#### Простые запросы

> methods: GET, POST, HEAD  
> headers:  
> - `Accept`  
> - `Accept-Language`  
> - `Content-Language`  
> - `Content-Type:`  
>   - `application/x-www-form-urlencoded`  
>   - `multipart/form-data`  
>   - `text/plain`  

> Простые запросы отправляются сразу с заголовком Origin, а для остальных запросов браузер  
> отправляет preflight-запрос (предзапрос), спрашивая у сервера разрешения.

> **Preflight запрос** - это запрос методом OPTIONS; содержит запрашиваемый метод и непростые  
> запрашиваемые заголовки.  
> `Access-Control-Request-Method`  
> `Access-Control-Request-Headers`  
>
> Сервер отвечает 200 OK (или нет) со списком разрешённых методов и заголовков, а также  
> время кеширования разрешения.

#### CORS error

> Ошибки возникают если отличается домен приложения и запрашиваемого ресурса.

> Для снятия CORS ограничений используются специальные заголовки:  
> - server:  
>   - `Access-Control-Allow-Origin`  
>   - `Access-Control-Allow-Methods`  
> - client:  
>   - `Access-Control-Request-Method`  
>   - `Access-Control-Request-Headers`  

___

