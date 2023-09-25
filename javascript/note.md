> [**no-plusplus**](https://eslint.org/docs/latest/rules/no-plusplus) - правило линтера, запрещает унарный плюс/минус, т.к. auto semicolon insertion может работать некорректно; предлагает использовать +=/-=

```for (let i = 0; i < 10; i += 1) {}```
___

> [**Object.hasOwn()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn) - замена Object.hasOwnProperty(); вернёт true если объект имеет собственное свойство, вернёт false если свойство наследованное или не существует в объекте.

```
const obj = {property: 'value'};
Object.hasOwn(obj, 'property'); // true

const arr = [1, 2, 3];
Object.hasOwn(arr, 2); // true
Object.hasOwn(arr, 3); // false элемента с индексом 3 нет
```

___

`.filter(Boolean)` - delete false elements from array; same as `.filter(el => Boolean(el))`.

___

`@deprecated` - для устаревших методов в jsDoc.

```
/**
* Some function
* @deprecated
*
* @param ...
*/
```

___

`trim()` - метод строки; возвращает новую строку без пробелов в начале/конце строки.

___

## Intl

> Встроенный класс для интернализации.  
> Очень мощная вещь для форматирования дат, чисел, валют и пр.

`Intl.Collator` - сравнение и сортировка строк.

`Intl.DateTimeFormat` - форматирование даты/времени.

`Intl.NumberFormat` - форматирование чисел.

___

## import/export

> Расширение при импорте не указывается.

`import { module as m } from '.'` - переименование при импорте.

```
// несколько имен для экспорта
export class Module {
  ...
}

export { Module as M };
```

> `export default` в модуле может быть только один, но при этом могут быть и другие не дефолтные экспорты.
> Импорт дефолтного экспорта пишется без {}, причём имя может быть любым.

```
// другой вариант экспорта
const a = 10;

export { a };

// пример импорта типа
const foo = (a: import('.').a) => {};
```

