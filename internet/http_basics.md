**HTTP** - протокол прикладного уровня; протокол обмена текстовыми данными по сети;  
не знает о компьютерах в сети; работает поверх установленного между компьютерами соединения (TCP).

___

**Дефолтные порты:**
- `80` - сайты доступные по http (по соглашению)
- `443` - по https

___

`x-` - в начале, указывает на кастомный заголовок.

___

## DNS

> **DNS** - domain name system; автоматизированная система соотношения/обновления ip-адресов и имён компьютеров.

`ip` - идентификатор устройства в сети.

`hosts.txt` - словарь; отвечает за привязку ip к имени компьютера;  
находится:  
- `windows/System32/drivers/etc/hosts.txt` в win
- `/etc/hosts` в nix

> **Домен** - символьное имя сервера в сети;  
> бывает:  
> - **корневой** - в url не используется, но подразумевается
> - **домен верхнего/первого уровня** - com, io, ru  
> - **домен второго уровня/основной** - имя сайта  
> - **поддомен** - 3го, 4го, и пр.

> **DNS-server** - система ответственная за хранение/актуализацию записей своих дочерних доменов.

> **Коневой DNS-server** - знает расположение ip, домена верхнего уровня.  
>  `dig` - для получения всех доступных dns записей.

## HTTP/1.1

> **Новое:**  
> - добавил виртуальные хосты;  
> - в запросе появилась строка `host` - определяет какой домен вернётся с ip-адреса (ip может содержать несколько доменов);  
> - добавил `Connection: keep-alive` - TCP-соединение на закрывается сразу;  
> - использование доменного имени (в HTTP/1.0 не использовалось доменное имя, его тогда не было, 1 ip-адрес соответствовал  
>   одному сайту).

`Default host` - иногда серверы отдают дефолтный хост, если в http/1.1 запросе не указан `host`.

`keep-alive` - экономит ресурсы, уменьшает нагрузку на процессор, уменьшает latency (ожидание);  
закрытие происходит или явно (передать заголовок `Connection: close`) или по таймауту (~30s).

___

## Body

> HTTP request/response могут содержать тело.

> Чтобы передать тело, нужен заголовок `Content-Length` в байтах;  
> часто нужен также `Content-Type: application/octet-stream` - для любого потока байт.

> Body можно отправить с любым глаголом, но для GET сервер игнорирует тело в запросе, для HEAD не отправит тело в ответе;  
> также если status=204 (no content), тело не отправится.

## Отправка форм

> Данные формы отправляются в теле запроса с заголовком:  
> - `Content-Type: application/x-www-form-urlencoded` - формат `key=value&key2=value`  
> - `Content-Type: application/json` - чаще используется json

> Содержимое запроса обычно кодируется клиентом (например `=` - `%3D`).

`Content-Type` - заголовок, в зависимости от которого сервер интерпретирует тело запроса.

## Transfer-encoding

`Transfer-encoding: chunked` - заголовок сервера, указывает что контент разбит на несколько кусков.

> Chunk приходит в теле ответа; сначала идёт число - длина чанка (обычно 400), затем кусок данных + перевод строки и т.д.;  
> последний чанк имеет длину 0.

> Если ответ сервера неизвестной длины, то можно отдавать его чанками.

___

## Query string

> Передаётся в строке запроса как GET, так и POST запросов.

`/user?name=Anna&age=40`

> GET запрос идемпотентный, поэтому query string тут нормально (и удобно при копировании ссылки).  
> 
> POST не идемпотентный, поэтому квери-параметры тут зло (например скопировал url, вставил в др вкладке  
> и создастся дубль ресурса).

> GET-запросы с квери-параметрами **кешируются**.  
> POST-запросы не кешируются.

> Поисковые индекс-боты ходят только по GET-ссылкам.

> GET + query string безопасно использовать пр отправке формы для выборки/фильтрации данных (например поисковый  
> запрос с google.com).  
> Этим мы не делаем изменения состояния данных сервера, можно поделиться ссылкой на поиск, кеширование.

___

## Redirect

> Редирект происходит например если сайт перешёл на https, тогда http запрос вернёт `300` + `Location`  
> заголовок и перейдёт по нему.

> Все 300 ответы имеют дополнительный заголовок `Location` с url, по которому браузер перейдёт автоматически. 

> Браузер отслеживает прекращает циклические редиректы.

## Basic Authentication

> У http есть базовая аутентификация - встроенная модалка с запросом в браузере. 
> При неправильном вводе или отмене, вернёт 401 Unauthorized.  
> После ввода логин/пароль, отправляет GET с заголовком `Authorization: Basic <userName/password_base64>`.

```
// для кодирования
printf 'user:pass' | base64
```

___

## Cookies

 > HTTP - stateless протокол, не хранит состояние.

> **Cookie** - механизм браузера для сохранения состояния (аутентификация, корзина и пр);  
> куки приходят с сервера, сохраняются в браузере и отправляются браузером обратно при последующих запросах;  
> срок создания устанавливается при создании;  
> объём не больше 4 кБ.


**Cookie бывают:**  
- **сессионные** - session cookie; без дополнительных параметров, хранятся в текущей сессии.
- **постоянные** - persistent cookie; имеют длину жизни, хранятся на диске

`Set-cookie` - заголовок устанавливает куки.

> Каждая кука посылается отдельным заголовком, содержит `key:value; domain=...`

`expires` - параметр, указывающий дату удаления куки.

`MAX-AGE` - то же что expires, но срок жизни указан в секундах; может указываться вместе с expires.

> Регистр имени куки не имеет значения.

`domain`, `path` - задают область видимости куки, т.е. url на который кука может отправляться; если не заданы,  
то кука будет пересылаться только для текущего пути и домена.

```
// кука будет работать для всех поддоменов site.com
// точка перед site.com - поддомены вида x.site.com
// / - корень сайта, т.е. кука будет отправлена для всех страниц
key:value; domain=.site.com; path=/;
```

> Чтобы **переустановить куку** должны совпадать key (имя куки), domain, path. Иначе создастся новая кука.

**Удаление куки:**  
- установить `MAX-AGE=0` или отрицательное значение  
- установить `expires` в прошлое

`HttpOnly` - дополнительный параметр куки, передавать куку только с AJAX-запросом, нельзя получить в js коде;  
для XSS безопасности.

> При обновлении страницы браузер добавляет заголовок `Cookie` с полученными куками в формате  
> `key:value; key2:value`.

`document.cookie` - доступ к кукам в js.

___

## Local/session storage

> **Local storage** - хранит данные бессрочно;  
> очищается с помощью js и очистки кеша браузера;  
> хранит до 5мБ;  
> работает по `Same origin policy` - т.е. сохранённые данные доступны доступны только для одного источника.

> **Session storage** - хранит данные одной текущей сессии;  
> очищается при закрытии браузера;  
> использует контекст браузера, каждая вкладка хранит свои данные;  
> объём больше чем cookie.

___


