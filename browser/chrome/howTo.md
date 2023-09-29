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


