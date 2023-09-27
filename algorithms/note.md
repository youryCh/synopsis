## Binary search

> **Важно** - данные должны быть упорядочены.

> **Суть алгоритма** - в отсортированном списке выбираем рандомный элемент (обычно ближе к середине),  
> если он равен искомому, то поиск завершён,  
> если он меньше искомого, то сам элемент и всё левее него отбрасываем,  
> если он больше искомого, то его и всё правее него отбрасываем,  
> дальше берём новый средний элемент из оставшегося списка.

```
function binarySearch(arr, x) {
  let l = 0;  // левая граница
  let r = arr.length - 1;  // правая граница
  let m = Math.round(Math.random() * (r - l) + l);  // рандомное значение между l и r

  while (l <= r && arr[m] !== x) {
    if (arr[m] < x) {
      l = m + 1;  // сдвигаем границу к центру, в зависимости от m
    } else {
      r = m - 1;
    }

    m = Math.round(Math.random() * (r - l) + l);
  }

  return m;
}
```

## Sorting

> Для понимания лучше поставить `debugger` и пройти по шагам в Source.

### Straight insertion (сортировка вставками)

```
function straightInsertion(arr) {
  let i, j, x;

  for (i = 1; i < arr.length; i++) {
    x = arr[i];
    j = i;

    while (j > 0 && arr[j - 1]) {
      arr[i] = arr[j - 1];
      j--;
    }

    arr[j] = x;
  }

  return arr;
}
```

### Straight selection (сортировка выбором)

```
function straightSelection(arr) {
  let i, j, k, x;

  for (i = 0; i < arr.length - 1; i++) {
    x = arr[i];
    k = i;

    for (j = i + 1; j < arr.length; j++) {
      if (arr[j] < x) {
        k = j;
        x = arr[k];
      }
    }

    arr[k] = arr[i];
    arr[i] = x;
  }

  return arr;
}
```

### Bubble sort (сортировка пузырьком)

> Суть в том что наименьшее число всплывает в начало массива.

```
function bubbleSort(arr) {
  let i, j, x;

  for (i = 1; i < arr.length; i++) {
    for (j = arr.length - 1; j > 0; j--) {
      if (arr[j - 1] > arr[j]) {
        x = arr[j - 1];
        arr[j - 1] = arr[j];
        arr[j] = x;
      }
    }
  }

  return arr;
}
```

___

