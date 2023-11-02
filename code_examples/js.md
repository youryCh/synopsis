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
