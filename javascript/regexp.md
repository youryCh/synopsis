# Регулярные выражения

`RegExp` - встроенный объект для работы с регулярками.

Два синтаксиса:
- инстанс класса RegExp
- литерал

```
(1)
const regexp = new RegExp('pattern', g);

(2)
const regexp = /pattern/gi;
```
___

## Флаги

- `i` - (ignore) игнорировать регистр
- `g` - (global) все вхождения (без этого флага только первое вхождение)
- `m` - многострочный режим
- `s` - режим dotall - точка соответствует символу перевода строки (`\n`)
- `u` - полная поддержка unicode (для корректной обработки суррогатных пар)
- `y` - режим поиска на конкретной позиции в тексте
___

## Поиск и замена

### match

`str.match(regexp)` - вернёт совпадения в виде массива или null при отсутствии совпадений.

**3 режима работы:**

(1)

Если передан флаг `g`, то возвращает массив всех совпадений.

```
'some string'.match(/st/gi);
```

(2)

Без флага `g` вернёт массив с единственным элементом; есть поля с дополнительной инфо.

```
const result = 'some string'.match(/st/i);

result.index;  // позиция совпадения
result.input;  // исходная строка
```

(3)

Если совпадений нет, то вернётся `null`, не зависимо от флагов.
___

### replace

`str.replace(regexp, replacement)` - заменяет совпадения с regexp строкой заменителем.

```
'We will, we will'.replace(/we/ig, 'I');  // 'I will, I will'
```

В строке replacement можно использовать **комбинации символов** для вставки фрагментов совпадения.

- ` $& ` - вставляет всё найденное совпадение
- ` $` ` - вставляет часть строки до совпадения
- ` $' ` - вставляет часть строки после совпадения
- ` $n ` - вставляет содержимое n скобочной группы регулярного выражения
- ` $<name> ` - вставляет содержимое скобочной группы с именем name
- ` $$ ` - вставить символ `$`

```
'I love HTML'.replace(/HTML/, '$& and JS');  // 'I love HTML and JS'

'John Doe'.replace(/(John)(Doe)/, '$2 $1');  // 'Doe John'

'John Doe'.replace(/John Doe/, 'Famous $&!');  // 'Famous John Doe!'
```
___

### Проверка test

`regexp.test(str)` - проверяет есть ли хотя бы одно совпадение; вернёт boolean.

```
const str = 'Some string';
const regexp = /st/i;

regexp.test(str);  // true
```
___

`*$` - для скобочных групп, проверять все символы с начала до конца строки.

```
/^[a-zA-Z0-9]*$/  // запретить все символя кроме англ букв, цифр, - и _
```
___

