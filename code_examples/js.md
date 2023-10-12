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


