`caret-color` - css свойство позволяет раскрасить курсор ввода текста.
___

## Rem

`1rem` - root element; по дефолту 16px.
___

Добавить отступ слева каждому соседнему элементу, кроме первого:

```
& + & {
 margin-left: 10px;
}
```
___

## user-select

Определяет поведение при выделении юзером; значения:  
- `none` - нельзя выделить
- `auto` - default; обычное вычисляемое выделение
- `text` - можно выделить текст
- `all` - выделить весь текст
- `contain` - выделить внутри границ элемента
___

## Container CSS

`container-type:`  
- `size`
- `inline-size`
- `normal`

```
<div class="container"></div>

// css
.container {
  container-type: inline-size;
}

// здесь контейнер работает как медиа запрос
@container (min-width: 400px) {  // условие применения стилей
  .card {
    display: grid;
  }
}
```

`container-name` - для именования нескольких контейнеров.

```
.container {
  container-type: inline-size;
  container-name: sidebar;
}

@container sidebar (min-width: 700px) {
  ...
}
```

### Новые единицы измерения

Работает как %, но от указанного контейнера.

- `cqw` - container query width
- `cqh` - cq height
- `cqi` - cq inline-size
- `cqb` - cq block-size
- `cqmin/cqmax` - min/max cqi or cqb

cqi можно использовать для font-size, чтобы размер текста изменялся относительно указанного контейнера (например  
для таблиц).
___


