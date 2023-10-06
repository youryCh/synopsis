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

___

> **Именование констант** - в нижнем регистре - для вычисляемых значений, в верхнем - для хардкода.

___

```
[1, 2, 3].every(Boolean); // true
[1, 2, 0].every(Boolean); // false
```

___

## Сумма чисел от 1 до n

`(n + 1) * n / 2`

___

`Number.POSITIVE_INFINITY` - лучше использовать вместо Infinity.

___

 ## Счётчик

 ```
 function counter() {
  let count = 0;

  return function() {
    return count++;
  };
 }

 const counter = (count = 0) => () => count++;  // не создаётся локальная переменная, а меняется
                                                // аргумент, что не всегда правильно
 ```

 ___

## Про js-движки

> Надо отличать движок Javascript (V8, SpiderMonkey) от движка браузера (Gecko, Blink).

> Js-движок служит для интерпретации, компиляции, оптимизации js кода;  
> он ничего не знает о DOM.

### Основные движки Js:
- **V8** - разработан Google; opensource; написан на С++;  
  используется в браузерных движках google, nodeJS.

- **SpiderMonkey** - первый js-движок; использовался в Netscape Navigator;
  сейчас используется в Firefox.
- **JavaScriptCore** - продаётся как Nitro; используется в Safari

___

> `flat(deep)` - метод массива; создаёт новый массив уменьшая вложенность.
> - `deep` - уровень вложенности, по дефолту == 1

```
[1, 2, [3]].flat();  // [1, 2, 3]

// убрать всю возможную вложенность
array.flat(Infinity);
```

___

> `flatMap(cb)` - метод массива; сначала применяет callback к каждому элементу (как map()),  
> затем преобразует в плоский массив (как flat()).

> Это как `arr.map((item) => [item]).flat()`

```
[1, 2, 3].flatMap((x) => [x * 2]);  // [2, 4, 6]

['it's sunny', 'in', ''].flatMap((x) => x.split(' '));  // ['it's', 'sunny', 'in', '']
                                                        // map бы вернул всё обёрнутое в массив
```

___

> `at()` - принимает индекс и возвращает элемент с этим индексом;  
> работает с любыми итерируемыми объектами и псевдомассивами;  
> по дефолту index == 0, может принимать отрицательные значения.

```
const lastPage = allPages.at(-1);

const arr = [1, 2, 3];
arr.at(-1);  // 3
             // раньше было arr[arr.length - 1]
```

___

> `bigint` - тип данных для целого числа произвольной длины.  
>
> Для создания bigint нужно добавить `n` в конце числа или через `BigInt(num)`.

___

Две формы typeof:
- `typeof x`  
- `typeof(x)`

___

`**` - оператор возведения в степень.

```
2 ** 2  // 4

2 ** (1 /2)  // квадратный корень

2 ** (1 / 3)  // кубический корень
```

___

## Логическое присваивание (ES2021)

`x &&= 10` - если x == true, то присвоится 10, если false, то останется прежнее значение;  
т.е. если у x есть значение, то присвоить новое.

`x ||= 10` - наоборот, если у x нет значения (falsy значения), то присвоить новое значение 10.

`x ??= 10` - присвоить если x равно null/undefined.

```
const obj = {
  a: 10,
  b: 0
};

obj.a ||= 20;  // obj.a == 10
obj.b ||= 30;  // obj.b == 30

obj.a &&= 30;  // obj.a == 30
obj.b &&= 10;  // obj.b == 0
```

___

## Array from Set

```
Array.from(new Set(additionalArr.map((id) => item.id)));
```

___

**Async код состоит:**
- инициализация
- разрешение

___

## includes() для строки
```
abc.includes('ab');  // true
```

___

## console methods

`console.time(name)` - стартует таймер.

`console.timeEnd(name)` - остановить таймер и вывести значение в мс.

`console.timeLog(name)` - выведет текущее значение без остановки таймера.

```
console.time('timer');  // start

el.onclick = () => {
  console.timeLog('timer');
  ...
  console.timeEnd('timer');
};
```

`console.trace()` - выведет стек вызова.

`console.count(label)` - считает количество вызовов.

`console.countReset()`

`console.group()` - группирует вывод.

`console.table()` - для табличных данных.

___

## Порядок в объекте

> Object.keys/values/entries не гарантирует порядок добавления при итерировании;  
> сначала идут ключи числа по возрастанию,  
> потом строки в порядке добавления  
> и символы в порядке добавления.

___


