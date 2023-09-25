**typeHinting** - подсказка типов в IDE.

___

`as const` - создаёт readonly tuple из массива.

```
// as const сделает tuple - массив ограниченной длины с конкретными свойствами
const args = [8, 5] as const;

// type 'hello'
const x = 'hello' as const;

// type { readonly text: 'text' }
const y = { text: 'text' } as const;

// type readonly[10, 20]
const z = [10, 20] as const;
```
___

