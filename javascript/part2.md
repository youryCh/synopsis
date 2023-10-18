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

`querySelectorAll` - работает с любым css селектором и псевдоклассом.

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


