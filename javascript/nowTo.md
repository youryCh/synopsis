## Если линтер требует default case

```
switch(...) {
  ...
  default:
    break;
}
```

___

## Разбить string и array

```
Array.from('str'); // [s, t, r]
// для любого итерируемого объекта и псевдомассива

[...'str']; // только для итерируемого объекта

'str'.split('');
```

> `Array.from()` - более гибкий, делает массив из чего угодно, плюс вторым аргументом можно  
> передать функцию-обработчик.
 ___

## Создать массив чисел 0-n

(1)
```
Array.from({ length: 5 }, (_, i) => i);
// 1й аргумент - псевдомассив, т.е. объект со свойством length
// 2й аргумент - функция преобразования, где i - индекс элемента
```

(2)
```
Array.from(new Array(5), (_, i) => i);
// тоже но создаём предварительный массив нужной длины
```

(3) Функция с возможностью преобразования элементов

```
const getArr = (length, fn = (i) => i) => Array.from({ length }, (_, i) => fn(i));

getArr(3, (i) => i + 1);  // [1, 2, 3]
getArr(3);  // [0, 1, 2]
```

___

## Использовать замыкание для filter

```
const inBetween = (a, b) => (num) => num >= a && num <= b;
const inArray = (arr) => (num) => arr.includes(num);
const array = [1, 2, 3, 4, 5];

array.filter(inBetween(2, 4));  // [2, 3, 4]
array.filter(inArray([2, 3]));  // [2, 3]
```

___

## Использовать optional chaining

```
items?.[item];

func?.()  // если func !== function, то вызова не будет
```

___

## Get query params

```
const urlParams = new URLsearchParams(window.location.search);
const id = urlParams.get('id');
```

___

## Определить размер объекта

`Object.keys(obj).length;`

___

## Сравнить объекты

```
const a = {};
const b = {};

a == b;  // false, т.к. сравнение идёт по ссылке (ссылки на переменные разные) а не по значению как с простыми типами
```
**Способы сравнения:**
- через JSON.stringify()
- lodash функция isEqual()

```
JSON.stringify(a) == JSON.stringify(b);  // true

_.isEqual(a, b);  // true
```
___

## Отменить выделение текста

```
el.addEventListener('mousedown', (e) => e.preventDefault());

// или
<p onmousedown="return false">Text</p>
```
___

## Отменить копирование текста

```
el.addEventListener('copy', (e) => e.preventDefault());

// или
<p oncopy="return false">Text</p>
```
___


