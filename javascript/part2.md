## DOM.document

**Окружение** - среда выполнения JS; предоставляет специфичную функциональность.

`window` - корневой объект браузерного окружения; является глобальным объектом для JS; представляет собой окно браузера  
и инструменты для работы с ним.
___

`DOM` - document object model; объектная модель документа, которая представляет содержимое страницы в виде объектов;  
по сути представление html документа в виде дерева.

`document` - основной объект для управления контентом.

DOM может использоваться не только в браузере, но и на сервере для работы с html.
___

`CSSOM` - css object model.
___

`BOM` - browser object model; дополнительные объекты для работы со всем кроме документа (navigation, location,  
alert/confirm/prompt, setTimeout и пр. WebAPI).
___

В браузере есть **автоисправление html**; дописывает теги и пр.

В `<table>` браузер всегда добавляет `<tbody>` внутри тега table, что надо учитывать.
___

**Типы нод:**
- document - DOM entry point
- elements - html tags
- text nodes - содержит текст (нижний уровень, не содержит детей)
- comments - комментарии внутри html
- doctype
- ещё 7 технических
___

**Как инспектить:**
1. в Elements открыть консоль -> `$0` - выделенная нода (`$0.style.color = 'red'`)
2. в консоли: `inspect(node)`
3. через `document.body.`
___

**Computed** в Elements - итоговый стиль с учётом применения встроенных стилей.
___

`document.documentElement` - самая верхняя нода (`<html>`).
___

**Дочерние узлы/дети** - элементы непосредственно вложенные в ноду.

**Потомки** - все вложенные элементы.

**Соседи** - ноды с одним и тем же родителем (например head и body).
___

**Списки нод** - покажут также текстовые/комментарии ноды:
- `childNode` - псевдомассив всех детей.
- `parentNode`
- `firstChild`, `lastChild`
- `.hasChildNodes()`

**Списки element-node:**
- `firstElementChild`
- `lastElementChild`
- `previousElementSibling`
- `parentElement`

```
document.documentElement.parentNode;  // document

document.documentElement.parentElement;  // null т.к. document не тег
```
___

**DOM-collections** доступны только для чтения (для изменения есть другие специальные методы).

Ссылки на DOM-коллекции изменяются автоматически, при изменении DOM.

Для перебора - `for of` (`for in` выводит лишние технические значения).
___

`children` - коллекция детей (только элементы).
___

**Навигационные ссылки table:**
- `table.rows` - коллекция строк tr
- `table/thead/tfoot.caption`
- `table.tBodies`
- `thead/tfoot/tbody.rows`
- `tr.cells` - ячейки
- `tr.sectionRowIndex` - номер строки текущей секции
- `tr.rowIndex` - номер строки в таблице
- `td.callIndex` - номер ячейки
___

### Поиск элементов

`getElementById`

`querySelectorAll` - работает с любым css селектором и псевдоклассом; возвращает статичную коллекцию.

`querySelector` - находит первый подходящий элемент.

```
document.querySelectorAll('ul > li:last-child');

// * - все элементы
```
___

**id как глобальная переменная:**

```
<div id="sss">...</div>
<script>
  sss.style.color = "green";  // если имя через тире - window['sss-id']
</script>
```
___

`matches()` - проверяет ноду на условие.

```
for (let el of document.body.children) {  // у коллекции children нет методов массива
  if (el.matches('div')) {
    ...
  }
}
```
___

`closest()` - ищет ближайшего предка, соответствующего переданному селектору (включая сам элемент); null - если не найдёт.

```
<ul class="book">
  <li class="chapter1">...</li>
</ul>

const chapter = document.querySelector('.chapter');

chapter.closest('.book');  // ul
```
___

Устаревшие методы поиска элементов:
- `getElementsByTagName()`
- `getElementsByClassName()` - возвращают коллекцию нод
- `getElementsByName()` - ищет по атрибуту `name`

Все эти методы возвращают **живую коллекцию** - изменяется если обновляется DOM.
___

`contains` - проверяет содержится ли в элементе искомый элемент.

```
elemA.contains(elemB);  // true/false
```
___

`console.log(document)` - в виде DOM дерева.

`console.dir(document)` - в виде объекта.
___

**IDL** - interface description language; язык спецификаций для описания интерфейсов; используется для описания классов DOM.
___

`elem.nodeType` - node type.

```
elem instanceof HTMLBodyElement  // same
```
___

`elem.tagName`  
`elem.nodeName` - returns tag name in upper case.
___

**Иерархия классов DOM нод:**
- EventTarget
- node:
  - Text
  - Comment
  - Element: 
    - SVGElement
    - HTMLElement:
      - HTMLBodyElement
      - HTMLInputElement
___

`element.innerHTML` - получить содержимое элемента в виде строки; можно присвоить новое содержимое.

Можно добавить тег script, появится в DOM, но не запустится.
___

`element.outerHTML` - весь html элемента.
___

`.nodeValue`  
`.data` - получить содержимое текстовых нод и нод-комментариев.

Содержимое комментариев используется в шаблонизаторах.

```
<!-- if isAdmin -->
...
<!-- /if -->
```
___

`textContent` - получить текст элемента без тегов; также можно присваивать.

При присваивании через `textContent`, теги внутри текста останутся строкой (полезно для ввода юзера). При присваивании через  
`innerHTML`, теги применятся.
___

 `element.hidden` - скрывает элемент (как `display: none`), но оставляет в DOM.

 ```
 // blinking
 setInterval(() => el.hidden = !el.hidden, 500);
 ```
 ___

 **Узнать свойства DOM элементов:**
 - html.spec.whatwg.org
 - `console.dir(element)`
 - DevTools -> Elements -> Properties
___

Ко всем атрибутам можно получить доступ, т.к. ноды это js-объекты.  
Имена html-атрибутов регистронезависимы (`'id' === 'ID'`).  
Значение атрибута всегда приводится к string.

**Методы доступа к атрибутам**:
- `el.attributes` - список всех атрибутов
- `el.hasAttribute(name)`
- `el.getAttribute()`
- `el.setAttribute(name, value)`
- `el.removeAttribute()`
___

`document.createElement(tag)` - создать новый элемент.

`document.createTextNode(text)` - то же для текстовой ноды.
___

**Методы вставки элементов:**
- `node.append(node|string)` - вставить элемент к конец ноды; принимает несколько элементов через зпт.
- `prepend()` - в начало ноды
- `before()` - до ноды
- `after()` - после ноды
- `replaceWith()` - заменит ноду переданной нодой/строкой.

```
div.before('text', document.createElement('span'));
// '<p>Hi</p>' - вставится как строка
```
___

`.insertAdjacentHTML(where, html)` - вставит html.
- `where`
  - `beforebegin`
  - `afterbegin`
  - `beforeend`
  - `afterend`

`insertAdjacentText()`  
`insertAdjacentElement()` - та ж хуйня.
___

`node.remove()` - удалить ноду.
___

`el.cloneNode(depth)` - клонирование элемента.
- `depth`
  - 'true' - глубокое клонирование
  - 'false' - поверхностное
___

`DocumentFragment` - пустая обёртка (как React.Fragment).

```
const fragment = new DocumentFragment();
```
___

**Устаревшие методы вставки:**
- `el.appendChild(node)`
- `el.insertBefore(node, nextSibling)` - вставляет ноду перед nextSibling
- `el.replaceChild(node, oldChild)`
- `el.removeChild(node)`
___

`document.write()` - загружает html-строку напрямую в document, минуя DOM; работает только во время загрузки html; работает  
очень быстро.
___
___

## Стили, классы


