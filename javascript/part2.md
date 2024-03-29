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

**Инлайн** стили используются если нужно работать со стилями динамически.
___

`className` - то же что class, сохранено т.к. слово class было зарезервировано, нельзя было использовать в объекте.
___

`el.classList` - специальный объект для работы с классами; имеет методы:
- `.add(class)`
- `.remove(class)`
- `.toggle(class)` - добавит класс если его нет, удалит если есть
- `.contains(class)` - проверит наличие класса

`classList` можно перебирать в `for of`.
___

`style` - объект соответствующий атрибуту style; не учитывает каскад; свойства с тире/подчеркиванием преобразует в  
camelCase.

```
el.style.backgroundColor = 'red';
```
___

**Сброс стиля** - нужно присвоить пустую строку, тогда браузер применит дефолтные стили.
___

`style.cssText` - для присваивания нескольких стилей одной строкой; затирает все стили элемента.

```
div.style.cssText = `
  color: red !important;
  width: 100px;
`;
```

То же можно сделать через `setAttribute('style', '...')`.
___

`getComputedStyle(element, [pseudo])` - вернёт объект со всеми применёнными стилями с учётом каскада; не покажет  
`:visited`.  
- `pseudo` - если нужен псевдоэлемент (например `::before`)

```
const allStyles = getComputedStyle(document.body);
```
___

`:visited` - псевдокласс применяется к уже посещённым ссылкам, меняя стиль; из-за приватности браузер ограничивает  
использование этого псевдокласса.
___
___

## Scroll

`el.offsetParent` - предок используемый при вычислении координат.

`el.offsetLeft/Right` - координаты относительно левого/правого верхнего угла offsetParent.

`el.offsetHeight/Width` - внешний размер элемента включая border; если элемент скрыт, то == 0.

```
// проверка на видимость
const isHidden = (el) => !el.offsetHidden && !el.offsetHeight;
```

`el.clientTop/Left` - отступы внутренней части элемента от внешней (обычно это размер border, кроме случая письма  
справа-налево).

`el.clientWidth/Height` - размер контентного блока с paddings, но с вычетом scroll.

`el.scrollWidth/Height` - общий размер элемента с учётом не видимой из-за прокрутки части; как и с clientWidth,  
включает паддинги, исключает scrollWidth (обычно 16px).

```
// разворачивание элемента
el.style.height = `${el.scrollHeight}px`;
```

`el.scrollLeft/Top` - размер прокрученной невидимой части элемента; свойство доступно для изменения (от левого  
верхнего угла).

```
// браузер прокрутит вверх или в конец
el.scrollTop = 0;
el.scrollTop = Infinity;
```
___

`document.documentElement` - корневой html элемент.

```
// размер общего контентного блока (за вычетом scrollbar)
documentElement.clientHeight
```
___

**Полная высота документа со скроллом:**

```
const scrollHeight = Math.max(
  document.body.scrollHeight,
  document.documentElement.scrollHeight,  // должно хватить этого только, остальное по историческим причинам
  document.body.offsetHeight,
  document.documentElement.offsetHeight,
  document.body.clientHeight,
  document.documentElement.clientHeight
);
```
___

**Получить текущий scroll:**
1. `documentElement.scrollTop/Left`
2. `window.pageYOffset/pageXOffset`
___

Для корректной работы прокрутки, html быть полностью загружен, для этого использовать script в конце body или  
аргумент defer.
___

**Прокрутить страницу:**
1. `document.documentElement.scrollTop/Left`
2. `document.body.scrollTop/Left`
3. `scrollBy(x, y)` - прокрутка относительно текущего положения
   `scrollTo(x, y)` - прокрутка на абсолютные координаты

```
// скролл в начало документа
window.scrollTo(0, 0);
```
___

`scrollIntoView(top)` - прокрутка страницы так чтобы элемент оказался вверху.  
- `top` - true - в верхнюю часть окна; false - внизу окна

```
el.scrollIntoView(true)
```
___

**Запретить прокрутку:**

```
document.body.style.overflow = 'hidden';
```

Чтобы контент не дёргался при исчезновении scrollbar, можно добавить padding.
___

## Позиционирование. Координаты

2 системы координат:
1. относительная:
   - `position: fixed;` - относительно верхнего левого угла текущего viewport
   - `position: relative;` - относительно ближайшего элемента с `absolute`
2. абсолютное:
   - `position: absolute;` - относительно верхнего левого угла всего документа

Если нет прокрутки, то эти координаты совпадают.
___

`el.getBoundingClientRect()` - вернёт объект DOMRect с координатами элемента относительно окна;  
свойства объекта:  
- `x/y` - начало прямоугольника относительно окна
- `width/height`
- `top/bottom` - верхняя/нижняя границы
- `left/right` - левая/правая граница
___

`el.elementFromPoint(x, y)` - вернёт элемент находящийся на этих координатах (самый глубоко вложенный); работает  
только с видимой частью окна, для остального вернёт null.

```
// получить элемент из центра экрана
const centerX = document.documentElement.clientWidth / 2;
const centerY = document.documentElement.clientHeight / 2;

const resEl = document.elementFromPoint(centerX, centerY);
```
___
___

## Events

**Событие** - сигнал от браузера что что-то произошло; обрабатываются асинхронно (например если во время выполнения  
click сработает ещё один click, то он будет ждать в очереди macrotask).
___

**Навесить обработчик можно:**

(1)

Обработчик внутри html тега с помощью атрибута `on<event_name>="..."`

```
<input onclick="clickHandler()" />  // все html атрибуты регистронезависимы
```

(2)

Навесить обработчик через свойство DOM элемента

```
<input id="elem" />

<script>
  elem.onclick = () => console.log('Hi!');  // DOM свойства чувствительны к регистру, в отличии от html тегов

  // удалить обработчик
  elem.onclick = null;
</script>
```

(3)

Назначить/удалить обработчик через специальные методы:
- `addEventListener()`
- `removeEventListener()`

`addEventListener` в отличии от других способов, позволяет навесить несколько обработчиков на одно событие. Также  
некоторые события слушаются только через addEventListener, например DOMContentLoaded.
___

`this` в хендлере:  
- в обычной функции - сам текущий элемент
- в стрелочной - window
___

`el.addEventListener(event, handler, [options])` - навесить обработчик на событие;  
- `event` - event name
- `handler` - функция или объект с методом `handleEvent(event)`; принимает аргументом `event` - объект содержит  
  подробности события; 
- `options:`
  - `once` - если true, то handler сам удалится после одного выполнения
  - `capture` - обработчик сработает на фазе погружения
  - `passive` - если true, то указывает что в обработчике не используется `preventDefault()`; позволяет браузеру  
  применять дефолтное поведение быстрее, даже до парсинга обработчика (например для уменьшения лагов при scroll).

Если третий аргумент указать как true/false, то это значение отнесётся к `capture` (по историческим причинам).

```
// несколько слушателей на одно событие
// при первом клике отработают все три алерта, затем только 2
input.onclick = () => alert(1);
input.addEventListener('click', () => alert(2));
input.addEventListener('click', () => alert(3), {once: true});

// объект-обработчик
el.addEventListener('click', {
  handleEvent(event) {  // при событии будет вызван метод с таким именем, если передан объект в handler
    alert(event.type);
  }
});

// указание браузеру что нет preventDefault
el.addEventListener('click', handler, {passive: true});
```
___

`el.removeEventListener(event, handler, [options])` - удалить обработчик события; должен принимать тот же handler,  
что передан в слушатель события.
___

**class-handler:**

```
class Menu {
  handleEvent(e) {
    const method = `on${e.type.at(0).toUpperCase()}${e.type.slice(1)}`;

    this[method](e);
  }

  onMousedown() {...}

  onMouseup() {...}
}

const menu = new Menu;

el.addEventListener('mousedown', menu);
el.addEventListener('mouseup', menu);
```
___

`DOMContentLoaded` - событие загрузки и построения DOM; слушать только через addEventListener.
___
___

### Всплытие событий

**Всплытие** - bubbling; обработчик сначала сработает на самом элементе, далее вверх по цепочке вложенности сработает  
на всех родительских элементах (если у них навешен обработчик); всплывают почти все события (focus не всплывает);  
большинство событий всплывают до window.
___

`event.target` - целевой элемент, на котором произошло событие (без учёта всплытия); доступен всегда через event.target.

`event.currentTarget` - целевой элемент с учётом всплытия; this == event.currentTarget.
___

`event.stopPropagation()` - остановит всплытие события на текущем обработчике; даст отработать всем обработчикам  
текущего элемента.

`event.stopImmediatePropagation()` - остановит не только всплытие события, но и работу всех остальных обработчиков  
текущего элемента, если они есть.
___

**3 фазы DOM event:**
1. capturing - захват, погружение; eventPhase: 1
2. target - целевой элемент; eventPhase: 2
3. bubbling - всплытие; eventPhase: 3

При клике по элементу, событие возникает у родителя, перехватывается (capturing), погружается до target, затем  
всплывает (bubbling) до родителя.

`event.eventPhase` - номер фазы события.
___

**Обработка события в фазе погружения:**

```
el.addEventListener('click', handler, true);
el.addEventListener('click', handler, {capture: true});  // то же
```
___

`event.target.closest()` - вернёт ближайшего предка.

```
event.target.closest('td');
```
___

`event.target.dataset` - доступ к дата-атрибутам целевого элемента.

```
<div data-name="element"></div>

if (event.target.dateset.name == 'element')
```
___

**Делегирование событий:**
1. вешаем обработчик на контейнер (общий родитель)
2. в обработчике проверяем event.target
3. для нужного элемента выполняем действие
___

`event.preventDefault()` - отмена дефолтного поведения браузера на событие (например переход по ссылке, отправка  
формы); доступно в addEventListener.

Если обработчик повешен через DOM свойство, отмена дефолтного поведения достигается через `return false;`.

```
el.onclick = () => {
  ...
  return false;
};
```
___

`event.target.getAttribute()` - получить значение атрибута.

```
event.target.getAttribute('href');
```
___

Некоторые события **автоматически срабатывают** вслед за определёнными событиями, например после `mousedown` на input  
сработает `focus`.
___

`event.defaultPrevented` - покажет было ли отменено где-либо в коде дефолтное поведения для данного события.

Полезно например для `contextmenu`, когда где-то в приложении нужно кастомное меню, а где-то дефолтное браузерное.
___
___

### Custom Event

`Event(type, options)` - встроенный класс для создания объекта кастомного события (лучше использовать  
специализированные классы кастомных событий, что позволяет использовать дополнительные опции); options по дефолту -  
всё false.  
- `type` - тип события
- `options:`
  - `bubbles` - boolean
  - `cancelable` - можно ли отменять дефолтное поведение
  - `composed` - будет ли всплывать за пределы shadow DOM
___

`el.dispatchEvent(event)` - запуск объекта события на клиенте; вернёт false если обработчик использует preventDefault.

```
<button id="btn"></button>

const clickEvent = new Event('click');  // лучше использовать MouseEvent или CustomEvent

btn.dispatchEvent(clickEvent);  // событие сработает как клик по кнопке
```
___

`event.isTrusted` - если true, то действие пользователя; если false, то событие сгенерировано из кода.
___

`CustomEvent(eventType, opt)` - встроенный класс для кастомных событий.
- `opt:`
  - `bubbles, cancelable, composed` - как у Event
  - `detail` - может содержать любую информацию; передаст в `event.detail`
___

**Специальные конструкторы событий:**
- `UIEvent`
- `FocusEvent`
- `MouseEvent`
- `WheelEvent`
- `KeyboardEvent`
- etc

Используются для конкретных типов кастомных событий. Имеют разные дополнительные поля.

```
const event = new MouseEvent('click', {
  bubbles: true,
  cancelable: true,
  clientX: 100,
  clientY: 200
});
```
___
___

### Mouse events

Бывают простые и комплексные (последовательность нескольких событий).

**Простые:**
- `mousedown/mouseup` - кнопка мыши нажата/отпущена над элементом
- `mouseover/mouseout` - курсор оказался над элементом, покинул элемент
- `mousemove` - любое движение курсора над элементом
- `contextmenu` - ПКМ или кнопка контекстного меню на клавиатуре (если есть)
- `copy` - копирование выделенного текста

**Комплексные:**
- `click` - состоит из mousedown и mouseup над тем же элементом
- `dbclick`
___

`event.which` - которая кнопка мыши была нажата:  
- `1` - ЛКМ
- `2` - средняя кнопка
- `3` - ПКМ
___

`event.shiftKey`  
`event.altKey`  
`event.ctrlKey`  
`event.metaKey` - вернёт true если при клике мыши нажата соответствующая клавиша (metaKey - cmd для Mac; altKey -  
также opt для Mac).
___

`clientX/clientY` - координаты курсора относительно окна браузера.

`pageX/pageY` - координаты курсора относительно всего документа; разница с clientX/clientY будет видна при прокрутке.
___

`event.clipboardData` - для работы с буфером обмена; содержит:  
- `files` - массив загруженных файлов
- `setData()` - поместить данные в буфер
___

`document.getSelection()` - вернёт объект выделения юзера.
___

**Пример с редактированием буфера:**

```
el.addEventListener('copy', (e) => {
  const selection = document.getSelection();

  e.clipboardData.setData(
    'text/plain',
    selection.toString().toUpperCase()
  );

  e.preventDefault();
});
```
___

`event.relatedTarget` - дополнительное свойство события mouseover/mouseout и mouseenter/mouseleave; для mouseover  
указывает на элемент с которого пришёл курсор, для mouseout указывает на элемент на который курсор перешёл;  
может быть null если курсор ушёл/пришел за окно браузера.
___

`mousemove` - событие движения мыши, не генерируется на каждое движение, а только периодически срабатывает (  
оптимизация).
___

`mouseenter/mouseleave` - по сути то же что mouseover/mouseout, но проще, так как не всплывает и переходы внутри  
элемента не считаются; не получится использовать делегирование (нет всплытия); имеет дополнительное свойство  
`relatedTarget`.
___
___

### Keyboard events

`keydown/keyup` - события нажатия/отпускания клавиши.

`event.key` - нажатый символ; для специальных клавиш вернёт почти то же что `code` - `Enter`, `Shift`, `F1`.

`event.code` - код клавиши на клаве (привязан к физическому расположению клавиши на клаве); не меняется при смене  
раскладки, регистра и пр.; буквы имеют вид `KeyA`, цифры - `Digit0`, остальные клавиши типа `Enter`, `Backspace`,  
`Tab`, `ShiftRight`, etc.

`event.repeat` - true когда сработает повтор символа при удержании клавиши нажатой.
___
___

### Pointer events

Новый общий тип событий для всех видов указателей, как мышь, стилус, тач. Не работает в старых браузерах.

Для событий мыши аналогичны, только вместо mousedown - pointerdown и так далее. По сути можно смело менять в коде  
mouseevent на pointerevent - для мыши всё останется как было, но с тач станет лучше работать (возможно где-то  
придётся добавить `touch-action: none;` в css).

**Список событий:**
- `pointerdown`
- `pointerup`
- `pointermove`
- `pointerover`
- `pointerout`
- `pointerenter`
- `pointerleave`
- `pointercancel` - последние 3 не имеют аналогов с mouse events
- `gotpointercapture` - генерируется при вызове `setPointerCapture()` для захвата target
- `lostpointercapture` - при освобождении от захвата target
- `pointercancel` - генерируется браузером при отмене pointer-события

Объект события содержит те же свойства что и mouse events, но есть дополнительные свойства:
- `pointerId` - чтобы идентифицировать несколько поинтеров (2 пальца)
- `pointerType:`
  - 'mouse'
  - 'pen'
  - 'touch'
- `isPrimary` - основной указатель (для multi touch)
- специфические для тач:
  - `width`
  - `height`
  - `pressure`
  - `tangentialPressure`
- для пера:
  - `tiltX`
  - `tiltY`
  - `twist`

`el.setPointerCapture(pointerId)` - привязывает target к элементу; полезно при dnd чтобы не вешать обработчик на  
весь документ и чтобы не срабатывали другие события (например при прохождении элемента над другой частью документа  
при перетаскивании); автоматически отменяется при pointerup или pointercancel.

`el.releasePointerCapture(pointerId)` - принудительное прекращение привязки target.
___
___

### Scroll events

`scroll` - событие прокрутки.

Прокрутка не отменяется через `preventDefault()` для события `scroll`. Можно отменить через CSS-свойство `overflow`,  
также через отмену дефолта на событие `keydown` для клавиш pageUp, pageDown.
___
___

## Forms

`document.forms[name/index]` - коллекция форм.

`form.elements[name/index]` - элементы формы (полученной через свойство выше).

`element.form` - каждый элемент формы хранит ссылку на форму.

**Доступ к значению:**
- `input.value`
- `textarea.value`
- `input.checked` - for checkbox
- `select.value`
___

### Select

[Doc](https://html.spec.whatwg.org/multipage/form-elements.html#the-select-element)

Свойства:
- `select.options` - options collection
- `select.value` - picked option
- `select.selectedIndex` - picked option index
- `select.options[0].selected` - default option

`multiple` - html-атрибут; позволяет выбрать несколько options одновременно.

```
<select id="select" multiple>
  <option value="1" selected>One</option>
  <option value="2" selected>Two</option>
  <option value="3">Three</option>
</select>
```

`new Option(text, value, defaultSelected, selected)` - создать option.
___

### Focus/blur

`focus` - событие генерируется когда юзер кликает по инпуту (или Tab); не всплывает, но погружается (т.е. на form  
не сработает событие focus, но сработает если слушатель ловит на фазе capture).

`blur` - когда инпут теряет фокус; данные обычно введены, их можно валидировать; всплытие - как у focus.
___

`autofocus` - html-атрибут; фокус сработает сразу после загрузки страницы.
___

`el.focus()`  
`el.blur()` - методы установки/снятия фокуса.
___

`tabindex` - html-атрибут; позволяет фокусироваться на элементах не поддерживающих фокус (div, li, span); принимает  
значения:  
- '1'-'n' - порядок важен для переключения через tab, имеет больший приоритет чем простые input, select
- '0' - приоритет общий с элементами без tabindex
- '-1' - возможна только фокусировка из кода (`el.focus()`)
___

`focusin`  
`focusout` - то же что focus/blur, но всплывают; только через addEventListener().
___

`document.activeElement` - получить текущий элемент с фокусом.
___
___

### Other events

`change` - событие завершения взаимодействия юзера; для текстового input - при переносе фокуса с него, для select,  
radio, checkbox - сразу при изменении значения.

`input` - событие каждого изменения значения input (в том числе при ctrl+v, вставке мышью, распознавание речи);  
событие не сработает если значение ввода равно предыдущему.

`cut/copy/paste` - события вырезания/копирования/вставки; относятся к классу `ClipboardEvent`; обеспечивают доступ  
к используемым данным через `event.clipboardDate`; в обработчике можно легко предотвратить копирование и пр. через  
`preventDefault()`.
___

`submit` - событие отправки формы; часто используется для валидации; генерируется при нажатии Enter в любом поле  
формы, либо при нажатии кнопки `<input type="submit" />`

`form.submit()` - метод отправки из кода данных формы.
___

`DOMContentLoaded` - событие document о загрузке html, DOM построен, внешние ресурсы (стили, img) могут быть ещё не загружены;  
срабатывает на document; скрипты выполнятся до события (кроме async скриптов и сгенерированных динамически скриптов);  
браузер автозаполняет формы после DOMContentLoaded.

`load` - событие window; как DOMContentLoaded событие, но загружены все внешние ресурсы; обработчик навешивается  
через `window.onload` или `addEventListener('load', f)`.

`unload` - событие window когда юзер покидает страницу (например отправка аналитики); используется  
вместе с `navigator.sendBeacon(url, data)` - отправляет данные (до 64 кб) в фоне (POST), не задерживая закрытие  
страницы.

```
window.addEventListener('unload', () => {
  navigator.sendBeacon('/analytics', JSON.stringify(data));
});
```

`beforeunload` - событие window; можно повесить обработчик window.onbeforeunload; если отменить дефолтное поведение,  
то браузер покажет модалку с подтверждением закрытия страницы.

```
window.onbeforeunload = () => false;
```
___

`document.readyState` - покажет текущее состояние загрузки html страницы; имеет 3 состояния:
- `loading` - документ загружается
- `interactive` - документ полностью распаршен (одновременно сработает `DOMContentLoaded`)
- `complete` - документ распершен и все внешние ресурсы загружены (одновременно сработает `load`)
___

`readystatechange` - событие document для отслеживания состояния загрузки html; то же что `readyState`.
___

`load`,  
`error` - события успешная загрузка/ошибка, для отслеживания загрузки сторонних ресурсов (script, iframe, img);  
error не отработает при рантайм ошибке скрипта (для этого window.onerror); для iframe для успеха и ошибки сработает  
load событие.

```
const script = document.createElement('script');

script.src = 'https://.../lodash.js';
document.head.append(script);

script.onload = () => {
  console.log(_.VERSION);  // здесь доступны переменные загруженного скрипта
};
```
___
___

## defer async

`defer` - (отложить) атрибут тега script (для внешних скриптов с `src`, иначе игнорируется); парсит скрипт  
параллельно с html, но выполняет после полного построения DOM, чтобы не блокировать рендер, до события  
DOMContentLoaded; сохраняет порядок скриптов.

`async` - атрибут script для асинхронной загрузки скрипта; не блокирует рендер; скрипт выполнится в порядке  
загрузки (не гарантирует порядок); полезно для загрузки внешних независимых скриптов; атрибут работает только для  
внешних скриптов (с src).

**Динамическая загрузка скрипта** - скрипт выполнится сразу как добавится в document; работает как async скрипт,  
если важен порядок, нужно явно отменять async.

```
const script = document.createElement('script');

script.src = './script.js';
script.async = false;  // отменит асинхронное поведение
document.body.append(script);
// или
document.head.append(script);
```
___

`crossorigin` - атрибут тега script для работы с CORS политиками; значения:
- без атрибута CORS запрещён
- ` anonymous` - доступ разрешён если сервер вернул заголовок `Access-Control-Allow-Origin: */our_domain`
- `use-credentials` - разрешает доступ и отправку кук и auth info, если сервер вернул  
`Access-Control-Allow-Origin: *` и `Access-Control-Allow-Credentials: true`.
___
___

## MutationObserver

`new MutationObserver(cb)` - встроенный класс; позволяет реагировать в ответ на изменение DOM (атрибуты, изменение  
элементов, содержания нод) и запускать переданный коллбек; полезно для работы со сторонними скриптами.
- `cb` - коллбек выполнится при изменениях; изменения получает первым аргументом - список объектов MutationRecord.

`.observe(node, config)` - статичный метод инстанса MutationObserver; принимает:  
- `node` - наблюдаемая нода
- `config` - объект с булевыми параметрами, определяет на какие изменения реагировать
  - `childList` - изменения в чилдах ноды
  - `subtree` - во всех потомках ноды
  - `attributes` - в атрибутах ноды
  - `attributeFilter` - массив конкретных атрибутов для наблюдения
  - `characterData` - наблюдать за `node.data` (для текстовых нод)
  - `characterDataOldValue` - передавать старое и новое значение `node.data` в коллбек
  - `attributeOldValue` - передавать старое и новое значение атрибута в коллбек

`.disconnect()` - прекратить наблюдение.

`.takeRecords()` - вернёт список необработанных коллбеком записей.

```
const observer = new MutationObserver(cb);

observer.observe(node, config);
```

`MutationRecord` - тип объекта, который получаем в коллбеке, переданном в конструктор MutationObserver; имеет  
свойства:  
- `type` - тип изменения:
  - `attributes` - изменён атрибут
  - `characterData` - изменены данные `el.data` (для текстовых нод)
  - `childList` - добавлены/удалены дочерние элементы
- `target`
- `addedNodes/removedNodes`
- `previousSibling/nextSibling`
- `attributeName/attributeNamespace` - (namespace для xml)
- `oldValue` - если включена `attributeOldValue`
___
___

## Выделение

### Selection, Range

`Range` - встроенный класс для работы с выделением текста в документе; задаёт границы диапазона.

Встроенные методы Range:
- `setStart(node, offset)` - установит начальную границу в позицию offset в node
- `setStartBefore(node)` - установит начальную границу перед node
- `setStartAfter(node)` - установит границу после node
- `setEnd(node, offset)` - тоже но для конечной границы
- `setEndBefore(node)`
- `setEndAfter(node)`
- `selectNode(node)` - выделить всю ноду
- `selectNodeContents(node)` - выделить всё содержимое ноды
- `collapse()` - схлопывает диапазон
- `cloneRange()`
- `deleteContents()`
- `extractContents()`
- `cloneContents()` - склонировать контент, вернуть как DocumentFragment
- `insertNode(node)` - вставить ноду в начале диапазона
- `surroundContents(node)` - обернуть node вокруг контента

```
const range = new Range();

range.setStart(span, 2);
range.setEnd(span, 4);

window.getSelection().addRange(range);
```
___

`Selection` - объект выделения; получить можно через `window.getSelection()`; anchor - начало выделения,  
focus - конец; focus может быть впереди anchor, если выделение справа-налево.

Свойства Selection:
- `anchorNode` - нода с которой началось выделение
- `anchorOffset` - смещение в начальной ноде
- `focusNode` - нода в которой закончилось выделение
- `focusOffset` - смещение в конечной ноде
- `isCollapsed` - true если диапазон отсутствует
- `rangeCount` - количество диапазонов (больше одного только в Firefox)

[Selection methods doc](https://learn.javascript.ru/selection-range)
___

`el.onselectstart` - событие начало выделения текста.

`document.onselectionchange` - событие изменения выделения.

`document.getSelection().toString()` - получить всё выделение.
___

### Выделение в формах

Для форм не нужны Range, Selection, есть свои встроенные свойства и методы:
- `input.selectionStart`
- `input.selectionEnd`
- `input.selectionDirection:`
  - `forward`
  - `backward`
  - `none`
- `input.select()` - выделить всё
- `input.setSelectionRange(start, end, [direction])` - изменить выделение
- `input.setRangeText(replacement, [start, end, selectionMode])` - заменить выделенное новым текстом
  - `selectionMode:` - `select`, `start`, `end`, `preserve`
___

`input.onselect` - событие завершения выделения.
___

**Как сделать невыделенным:**
1. CSS-свойство `user-select: none`
2. предотвратить дефолтное поведение: `el.onselectstart = () => false`
3. стереть значение после выделения: `document.getSelection().empty()`
___
___

## Event loop

**Event loop** - по сути бесконечный цикл JS движка, ожидает задачи, формирует из них очередь, выполняет по  
принципу FIFO и снова ждёт.

Рендер не происходит во время выполнения task.

Долгая задача блокирует очередь, через время браузер предлагает убить её.

**Microtask** - генерируются в коде; микрозадачи генерируются с помощью:  
- `.then`, `.catch`, `.finally`
- `await`
- `queueMicrotask()`
___

`setTimeout(f, 0)` - добавить задачу в очередь макрозадач.

`queueMicrotask(f)` - добавить задачу в очередь микротаск.
___

`Web Workers` - выполнение кода в параллельном потоке, со своим event loop и переменными окружения; не имеет  
доступа к DOM; можно обмениваться сообщениями с основным потоком; можно использовать для тяжёлых задач.
___
___
