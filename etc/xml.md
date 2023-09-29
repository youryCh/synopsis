> **XML** - eXtensible Markup Language; расширяемый язык разметки; используется для хранения, передачи,  
> представления данных.

`<?xml version="1.0" encoding="windows-1251"?>` - xml декларация.

> xml должен иметь **корневой элемент**, идёт после декларации.

> Теги регистрозависемы (`<a/> != <A/>`).

`<` и `&` - строго запрещены, заменять на код.

Код для замена некоторых символов:
- `&lt` - <
- `&gt` - >
- `&amp` - &
- `&apos` - '
- `&quot` - "

`<!-- comment -->` - комментирование как в html.

> **Имя тега** должно начинаться с буквы (кроме xml) или _

### Namespaces

(1)
> **Префикс** - позволяет избежать конфликта имён.

```
<a:table>...</a:table>
<b:table>...</b:table>

// все дочерние элементы должны содержать тот же префикс
```

(2)
> **xmlns** - атрибут для namespace, внутри обычно url - ссылка на информацию о namespace или любой уникальный текст.

```
<h:table xmlns:h="https://...">
```

(3)
> Атрибут в root-элементе.

```
<root xmlns:h="..." xmlns:f="...">
  <h:table>...</h:table>
  <f:table>...</f:table>
</root>
```

(4)
> **default namespace** - позволяет не писать префиксы всем чилдам.

```
<table xmlns="...">
```

___

> **XSLT** - язык для трансформации xml в html.

___

## XML DOM

> **DOMParser** - встроенный класс браузера, содержит метод parseFromString(string, type), позволяет парсить xml (html, SVG)  
> из строки в DOM.

```
const parser = new DOMParser();
const doc = parser.parseFromString('<title>Test</title>', text/xml);
el.innerHTML = doc;
```

> **XML DOM** - интерфейс для работы с xml.

> **xmlDoc** - как document в html.

> **XPath** - синтаксис для навигации/поиска элементов в xml.

___


