## includes() для строки
```
abc.includes('ab');  // true
```
___

## Array from Set

```
Array.from(new Set(additionalArr.map((id) => item.id)));
```
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

## Сумма чисел от 1 до n

`(n + 1) * n / 2`
___

```
[1, 2, 3].every(Boolean); // true
[1, 2, 0].every(Boolean); // false
```
___

`.filter(Boolean)` - delete false elements from array; same as `.filter(el => Boolean(el))`.
___

## Ахеренный приём присваивания

```
let a, b;

[a,, b] = 'aaa bbb ccc'.split(' ');

a;  // 'aaa'
b;  // 'ccc'
```
___

## Найти min/max число в массиве

```
Math.min.apply(null, arr);

Math.max.apply(null, arr);
```
___

## Factorial

**Факториал числа** - произведение натуральных чисел от 1 до самого числа (включая данное число); факториал 0 и 1  
равен 1.

```
// через рекурсию
function factorial(n) {
  if (n !== 1) {
    return n * factorial(n - 1);
  } else {
    return 1;
  }
}

// через стрелочную функцию
const fact = (n) => n === 0 ? 1 : n * fact(n - 1);

// через цикл
function factorial2(n) {
  let arr = [];
  let result = 1;

  for (let i = 0; i < n; i++) {
    arr[i] = i + 1;
  }

  for (let i = 0; i < arr.length; i++) {
    result *= arr[i];
  }

  return result;
}

```
___

## Compare objects

```
const getObjectsDifference = (firstObject, secondObject) => Object
  .entries(firstObject)
  .reduce((acc, [ key, value ]) => {
    const result = isEqual(value, secondObject[key]) ? {} : {[key]: secondObject[key]};

    return {...acc, ...result};
  });
```
___

## Env usage. Zustand

```
import { create, StateCreator, UseBoundStore, StoreApi } from 'zustand';
import { devtools } from 'zustand/middleware';

export const createStore = <T>(
  fn: StateCreator<T>,
  name: string,
): UseBoundStore<StoreApi<T>> => {
  if (process.env.NODE_ENV === 'development') {
    return create(
      devtools(fn, { name }),
    );
  }

  return create(fn);
};
```
___

## Get yesterday

```
export const getYesterday = (): Date => {
  const date = new Date();

  date.setDate(date.gatDate() - 1);

  return date;
};
```
___

## Reduce with Map

```
const getRowsMap: GetRowsMap = (graphics) => graphics.reduce(
  (acc, g) => acc.set(g.shift, [...(acc.get(g.shift) || []), g]),
  new Map<SHIFT, ChoiceGraphicResponse[]>(),
);
```
___

## String normalizer

```
const normalizeString = (string) => {
  const symbolsMap = new Map([
    ['&', '&amp;'],
    ['<', '&lt;'],
    ['>', '&gt;'],
    ['"', '&quot;'],
    ["'", '&#39;']
  ]);
  const regexp = /[&<>"']/g;
  const hasSymbols = RegExp(regexp.source).test(string);

  return string && hasSymbols
    ? string.replace(regexp, (char) => symbolsMap.get(char))
    : string || '';
};
```
___

## Фильтр дублей массива

```
arr.filter((el, idx, arr) => arr.indexOf(el) === idx);
```
___

## Dynamic optional fields in object

```
{
 name: John,
 surname: Johnson,
 ...(age && {age: 44})
}
```
___

## Flatten

```
const flatten = <T>(array: T[][]): T[] => Array.prototype.concat.apply([], array);
```
___

## Remove object fields

```
type TRemoveFields<T, K extends (keyof T)[]> = Omit<T, K[number]>;

const removeObjectFields = <T extends object, K extends (keyof T)[]>(
  object: T,
  fields: K
): TRemoveFields<T, K> =>
  Object.fromEntries(Object.entries(object).filter(([key]) => !fields.includes(key as (keyof T)))) as T;
```
___

## Capitalize function

```
const capitalize = (word) => word.replace(
 /\w+/g,
 ([first, ...rest]) => `${first.toUpperCase()}${rest.join('').toLowerCase()}`
);
```
___
