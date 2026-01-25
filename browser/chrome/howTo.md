## Запустить Chrome с отключенной CORS защитой

**Два варианта:**
1. через командную строку открыть новое окно
2. создать отдельный ярлык с постоянным флагом игнорирования

(1)
- `WIN+R` - открыть cmd
- `chrome.exe --user-data-dir="C://Chrome dev session" --disable-web-security`  - вставить в cmd и enter

> Откроет окно хром с отключенными cors.

(2)
- создать отдельный ярлык chrome
- ПКМ -> Properties -> Shortcut вкладка
- в поле `Target` в конце строки через пробел ввести `--disable-web-security --disable-gpu --user-data-dir=~/chromeTemp`
___

## Запустить Chrome с игнорированием ошибок сертификата

- создать отдельный ярлык
- в свойствах Target дописать в конец после пробела `--ignore-certificate-errors`
___

## Clear browser data

`CTRL+SHIFT+DEL` или если в инкогнито, то просто закрыть окно.
___

## Починить ошибку "This site can't provide a secure connection"

- `chrome://net-internals/#hsts` в строке браузера
- в `Delete domain security policies` указать домен (например localhost) и нажать Delete

**HSTS** - is HTTP Strict Transport Security: a way for sites to elect to always use HTTPS.
___

## Add SSL certificate

`chrome://settings/security` -> Manage certificates -> Import
___

## Save HTML page

`CTRL+S` just on page
___

## Disable AI trash

1. `chrome://flags/` - find 'ai' and disable
2. `https://github.com/corbindavenport/just-the-browser?tab=readme-ov-file` edit registry instructions, add some helpfull policies
3. `chrome://policy/` - check policies
___
