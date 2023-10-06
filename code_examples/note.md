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
export const getRowsMap: GetRowsMap = (graphics) => graphics.reduce(
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


