# Styled Components

Библиотека для работы со стилями в react приложениях; использует css-in-js подход; не стилизует html элементы, а  
react компоненты в целом (определяет стилизованные компоненты со своими инкапсулированными стилями); автоматически  
расставляет вендерные префиксы.

`yarn add styled-components` - установка.

```
import styled from 'styled-components';

const Button = styled.button`  // использует теговые шаблоны es6
  font-size: 2em;
`;
```

```
const Input = styled.input`
  color: grey;
  border: none;

  ::placeholder {  // цвет плейсхолдера в инпуте
    color: red;
  }
`;
```
___

## Кастомизация пропсами

`$prop` - добавить `$` перед именем пропа, если нужно чтобы проп не попал в DOM.

```
const Button = styled.button`
  color: ${(props) => props.primary ? 'white' : 'black'};
  ${({ isActive }) => isActive && `color: ${active};`}
`;
```
___

## Расширение стилей компонента

`styled(Component)` - если нужно создать компонент с другими стилями.

```
const Button = styled.button`
  color: green;
`;

const TomatoButton = styled(Button)`
  color: 'tomato';
`;
```

`as` -  (withComponent раньше был) проп для динамической замены элемента.

```
const Btn = styled.button`
  color: ${({ theme }) => theme.color};
`;

<Btn as="a">Click me!</Btn>  // изменит <button> на <a> с теми же стилями
```

```
const ReverseButton = (props) => {
  return <Button {...props} children={props.children.split('').reverse().join('')} />
};

<Button>Normal</Button>
<Button as={ReversedButton}>Reversed</Button>
```
___

## Стандартные атрибуты тегов

`styled.attrs()` - для указания атрибутов html тегов.

```
const Input = styled.input.attrs({
  type: 'text',
  placeholder: 'Write here'
})`
  font-size: 1rem;
`;
```

Пример с Bootstrap классами:
```
const PrimaryBtn = styled.button.attrs({
  className: 'btn btn-primary'
})`
  outline: none;
`;
```
___
