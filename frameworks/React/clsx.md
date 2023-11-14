# clsx

Библиотека создания сложных составных css-классов, классов с условиями.

`clsx(...input)` - функция принимает css-классы строкой через зпт, объектом/объектами, массивом/массивами и вообще  
вперемежку; возвращает строку вида 'class1 class2 class3'.

`yarn add clsx` - установка.

```
import clsx from 'clsx';

<Button className={clsx('someClass', isTrue && 'anotherClass')} />

<div className={clsx('row', classFromProps)}></div>
```
___
