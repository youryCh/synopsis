### Event typing

`onChange(event: React.ChangeEvent<HTMLInputElement>){}`  

`React.FormEvent` и ещё куча других, смотри доку.

___

## eslintrc.js

> После создания react проекта через cra/npm run eject, создать конфиг-файл линтера вручную:  
> `npm init @eslint/config`

___

**VFC** - Void Function Component; type for react component without children.

**FC** - Function Component; type fjr react component with children.

___

> **Hydration** - гидратация; это когда React связывает отрисованные DOM-элементы с js-кодом,  
> т.е. делает UI интерактивным.

___

## SOLID in React

> SOLID позволяет писать более читабельный, расширяемый, поддерживаемый и тестируемый код.

### **SRP - Single Responsibility Principle**
> Every class should have only one responsibility (i.e. do exactly only one thing).
> 
> В React это относится к компонентам и функциям.  
> По сути это про разбиение большого компонента на более мелкие по логическому принципу.

> Пример:  
> Если в компоненте есть state и useEffect, это можно вынести в кастомный хук.

> SRP также улучшает производительность, т.к. разбивает long task на небольшие задачи,  
> этим избегаем блокировки main thread браузера.

### **OCP - Open-Closed Principle**

> Software entities should be open for extension, but closed for modification.
>
> Если для расширения функционала компонента приходится его изменять, значит компонент  
> нарушает OCP.

> Пример:  
> Можно вынести изменяемую часть в children.

### **LSP - Liskov Substitution Principle**

> Subtype objects should be substitutable for supertype objects.
>
> Типа если компонент содержит input, то он должен расширять интерфейс инпута.

```
// Пример:
interface Props extends InputHTMLAttributes {
  isLarge?: boolean;
}

const SearchInput = (props: Props) => {
  const { isLarge, ...restProps } = props;

  return <Input { ...restProps } />;
};
```

### **ISP - Interface Segregation Principle**

> Clients should not depend upon interfaces that they don't use.
>
> React-компонент не должны зависеть от props, которые он не использует.

> Пример:  
> Использовать ...restProps для передачи в children.

### **DIP - Dependency Inversion Principle**

> An entity should depend upon abstractions, not concretions.
>
> Делать компоненты более абстрактными и независимыми и позволять им расширяться, а не изменяться.

> Пример:  
> В форме не должен быть захардкожен url - это конкретная зависимость, может поменяться.  
> Чтобы форма зависела от абстракции - передать через пропсы url/handler с url  
> Можно передавать зависимости в конструктор класса.

___

## Lazy loading

`<link rel="preload" />` - средний приоритет; начинает загружаться вместе с родителем.

`<link rel="prefetch" />` - низкий приоритет; загружается после того как завершится загрузка родительского ресурса.

```
//в React
const About = lazy(() => import(/* webpackPrefetch: true */ './About'));
                                // webpackPreload  
```

___


