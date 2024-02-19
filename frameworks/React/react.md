**React** - кроссплатформенный фреймворк для создания SPA приложений (html файл получаем 1 раз с сервера, дальше  
контент генерируется динамически).

**Плюсы SPA:**
- простая реализация бизнес логики
- быстро работает
- экономит трафик
- легкие запросы на сервер, но их много
- легко реализовать кеширование приложения

**Минусы SPA:**
- большой TTI (time to interactive)
- проблемы с SEO
___
___

## Презентационные и контейнеры компоненты

Паттерн разделения сложного компонента.

**Презентационный компонент** - глупый компонент; только для отображения переданных ему данных; содержит разметку  
и стили; свойства:
- отвечает за внешний вид
- не зависит от других частей приложения (в т.ч. от Redux)
- содержит свою разметку и стили
- не содержит состояние
- данные и callbacks получает через пропсы

**Контейнер** - компонент с бизнес-логикой (запрос, обработка данных); является обёрткой над глупыми компонентами;  
рендерит презентационные компоненты, передавая им необходимые данные пропсами; свойства:
- отвечает за логику работы с данными
- может быть связан с внешними источниками данных
- не содержит DOM элементов и стилей
- может иметь состояние
- передаёт данные презентационным компонентам
___

## Создание приложения

**React компонент** - функция или класс (наследующий от React.Component и реализующий метод render), возвращающая  
react-элемент или null.
___

### create-react-app

Библиотека для быстрой первоначальной настройки react-приложения; делает дефолтные настройки  
webpack, babel, jest, eslint, etc.

`npx create-react-app proj_name` - создание react-приложения.
___

### ReactDOM

`ReactDOM` - библиотека для общения React с DOM браузера; синхронизирует VirtualDOM и DOM (reconciliation);  
содержит метод render() - вызывается 1 раз для рендера точки входа приложения.

`ReactDOM.render(what, where)`

```
// создание элемента через jsx синтаксис
// className т.к. class зарезервированное слово в js
const element = <h1 className="element">Hello</h1>;

// после babel jsx код транспилируется в это:
const element = React.createElement(
  'h1',
  {className: 'element'},
  'Hello'
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

**JSX** - специальный js синтаксис; позволяет использовать элементы DOM с прокидыванием атрибутов; синтаксический  
сахар над `React.createElement()`; с помощью babel транспилируется в js код.

`React.createElement(tag, attributes, value)` - создание элемента; при использовании jsx синтаксиса, происходит  
под капотом через babel.

`{}` - для передачи значений внутри jsx (как во Vue).

```
function Example(props) {
  return <p>{props.text}</p>
}
```

Передача пропсов в компонент:

```
const name = 'Anna';

ReactDOM.render(
  <React.StrictMode>
    <App name={name} />
  </React.StrictMode>,
  document.getElementById('root')
);
```

В классовом компоненте к пропсам обращаться через `this`:

```
<p>{this.props.name}</p>
```
___

`index.html` - точка входа spa приложения; содержит div id="root".
___

`reportWebVitals()` - для сбора статистики.

```
// выведет статистику в консоль
reportWebVitals(console.log);
```
___

`npm run eject` - разбирает cra проект, открывает доступ к скрытым по дефолту конфигам webpack, babel.
___

## Фреймворк или библиотека?

**Фреймворк** - реализация всей архитектуры приложения; все решения основываются на подходах продиктованных  
фреймворком; фреймворк диктует как писать код (например Angular).

**Библиотека** - набор инструментов для реализации частной задачи; подход распространяется на реализацию отдельного  
компонента, не всего приложения.

React сам по себе это библиотека для реактивного рендера в DOM, но обмазанный кучей доп библиотек по сути становится  
фреймворком.

**Императивный стиль** - описывает шаги КАК добиться желаемого результата.

**Декларативный стиль** - описание какой результат нужен.

React использует декларативный подход - мы указываем какое состояние должен иметь интерфейс и react сам заботится  
о достижении требуемого состояния.
___
___

## Component lifecycle

**Этапы жизненного цикла:**
1. mount - монтирование; добавление компонента на страницу в первый раз
2. update - обновление; компонент существует, но обновились данные от которых он зависит (state, props)
3. unmount - размонтирование; компонент существует и происходит его удаление из DOM

### Методы жизненного цикла

На каждом этапе жизненного цикла React сам вызывает соответствующий метод жизненного цикла (классовые компоненты)  
или callback хуков (функциональные компоненты).

`componentDidMount()` - вызывается после рендера в DOM; здесь обычно отправляются запросы на сервер, устанавливаются  
таймеры, подписка на события.

`componentDidUpdate()` - вызывается при обновлении state/props после ререндера; первые 2 аргумента state, props до  
обновления; здесь можно выполнить сравнение состояния и пропсов до и после апдейта.

```
componentDidUpdate(prevProps) {
  if (this.props.userId !== prevProps.userId) {
    this.fetchData(this.fetchUserData(this.props.userId));
  }
}
```

`componentWillUnmount()` - вызывается перед удалением компонента из памяти; здесь можно отменить ранее отправленные  
запросы, таймеры, подписки на события (чтобы избежать утечки памяти).

```
---------------------------------------------------
MOUNT                  constructor()
                            v
                          render()
                            v
                    componentDidMount()
                            v
UPDATE   (new props | setState() | forceUpdate())
                            v
                          render()
                            v
                    componentDidUpdate()
                            v
UNMOUNT            componentWillUnmount()
---------------------------------------------------
```

```
// пример порядка выполнения
class App extends React.Component {
  constructor(props) {
    super(props);  // 1
  }

  componentDidMount() {}  // 3

  render() {}  // 2
}
```

Если есть дочерний компонент, то порядок такой:
1. constructor
2. render
3. child constructor
4. child render
5. child componentDidMount
6. componentDidMount

`getDerivedStateFromProps(props, state)` - (derived - производное) для установки initial state из props; вызывается  всегда перед render().

`getSnapshotBeforeUpdate()` - позволяет брать информацию DOM перед возможным изменением; вызывается перед  
фиксированием.
___
___

## Хуки

Специальные функции, позволяют работать с состоянием и жизненным циклом компонентов.  
Имена хуков, в т.ч. кастомных, начинаются с `use` - указывает реакту что эта функция - хук.  
Хуки появились в 16 версии.

**Правила хуков:**
1. Хуки вызываются на верхнем уровне компонента или из других хуков, не из вложенных функций.
2. Нельзя использовать хуки с условными операторами, в циклах и после условного return.
3. В зависимостях нужно указывать все переменные, которые используются в коллбеке (исключения: переменные вне тела  
   компонента, функция-сеттер из useState).

### useEffect

В функциональных компонентах нет методов жизненного цикла, этот функционал достигается хуком useEffect.  
Позволяет наблюдать за изменением (например переменной) и выполнять какую-то логику (эффект).

`useEffect(cb, [dependencies])` - cb будет вызван (async) при:  
1. монтировании компонента
2. изменении одной из зависимостей

`useEffect(cb, [])` - cb выполнится только раз при монтирования компонента; аналог **componentDidMount**.

`useEffect(cb)` - cb выполнится при монтировании и каждом рендере; аналог **componentDidUpdate** (но он  
не сработает при первом рендере); чтобы получить полный аналог componentDidUpdate, надо использовать useEffect  
вместе с useRef (см 320) или useLayoutEffect.

`useEffect(() => cb)` - возвращаемый cb будет выполнен при размонтировании; аналог **componentWillUnmount**.

```
useEffect(() => {
  const handleResize = () => setWidth(window.innerWidth);

  window.addEventListener('resize', handleResize);

  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);
```

useEffect может вернуть только функцию, поэтому нельзя объявить async useEffect (т.к. async функция вернёт промис).  
Но можно объявить отдельную async функцию и вызвать её внутри useEffect.

**Разница между useEffect(cb, []) и componentDidMount():**

(1) ComponentDidMount:

```
                            render()
                               v
                 componentDidMount(newState)
                               v
  (код метода выполняется синхронно, задерживая следующий рендер)
                               v
                            render()
```

(2) useEffect(cb, []):

Сам cb выполняется асинхронно и не блокирует рендер. Т.е. ui отрисуется (с initial values) до того как сработает  
callback useEffect.  
В классовом компоненте такое поведение можно сымитировать так:

```
componentDidMount() {
  setTimeout(() => {});  // 0 - default delay
}
```
___

### useLayoutEffect

`useLayoutEffect(cb, [deps])` - всё так же как у useEffect, разница только в том что callback выполнится синхронно,  
т.е. задержит рендер браузера (также как componentDidMount).
___

### useCallback

`useCallback(cb, [deps])` - используется для мемоизации коллбеков, передаваемых в дочерние компоненты; также  
рекомендуется оборачивать функции возвращаемые кастомным хуком; callback запоминается и пересоздаётся только  
при изменении зависимостей; возвращает переданный callback.

Зачем оно надо:

```
const Cars = () => {
  const handler = useCallback(() => {
    console.log('click');
  }, []);

  return [1, 2, 3].map(
    (car, idx) => <Car key={idx} car={car} onClick={handler}>
  );
};
```

При ререндере Cars, ссылка на handler не меняется (т.к. запоминается useCallback), поэтому не произойдёт ререндер  
дочерних компонентов Car.  
Если не оборачивать handler, то при ререндере Cars, handler создастся каждый раз заново, и Car, видя новую ссылку  
на handler, перерендерится.
___

### useMemo

`useMemo(cb, [deps])` - выполняет cb и мемоизирует результат; возвращает результат выполнения cb;  

Коллбек, переданный в useMemo будет выполнен при первом рендере и изменении зависимостей. Используется для  
тяжёлых функций, чтобы не вычислять при каждом рендере. Не рекомендуется использовать другие хуки внутри  
useMemo (особенно useState - может зациклить).
___

### useRef

`useRef(currentInitial)` - возвращает ref, который сохраняется в течении всего жизненного цикла компонента.

`ref` - специальный объект с мутируемым свойством `current`; объект ref не изменяется, current можно изменять  
не вызывая обновление компонента

Использование:
- получение ссылки на DOM элемент (например навесить focus на input) или на другой компонент
- получение состояния предыдущего рендера
- для взаимодействия с DOM элементом напрямую
- сохранение переменных, не влияющих на отображение компоненты

```
// пример кастомного хука
const usePrevious(value) => {
  const ref = useRef();

  useEffect(() => {
    ref.current = value;
  }, [value]);

  return ref.current;
};

// в классовых компонентах доступно через componentDidUpdate(prevProps, prevState)
```
Каждый DOM элемент в React имеет атрибут `ref`, в который можно передать ref-объект. Обычно такое прямое 
взаимодействие с DOM используется для focus, scroll.

```
// пример прямого взаимодействия с DOM
const Input = (props) => {
  const inputRef = useRef(null);

  const handleClick = () => {
    inputRef.current.focus();
    inputRef.current.style.backgroundColor = 'red';  // можно работать как обычным DOM элементом

    console.log(inputRef.current);  // <input>
  };

  useEffect(() => {
    inputRef.current?.focus();  // взаимодействие с элементом через ref
  }, []);                       // focus() - стандартная функция webAPI

  return (
    <input ref={inputRef}>
    <button onClick={handleClick}>Click me</button>
  );
};
```

Если нужно выполнить callback при обновлении компонента, но не при первом рендере, можно использовать ref.

```
const isFirstRender = useRef(true);

useEffect(() => {
  if (!isFirstRender.current) {
    console.log('Этот текст виден каждый рендер, кроме первого');
  }
});

useEffect(() => {
  isFirstRender.current = false;
}, []);
```
___

### useState

`useState(initial)` - возвращает массив, содержащий объект состояния и функцию для его изменения.

```
const [state, setState] = useState({});
```

`state` - объект описывающий состояние компонента; при изменении state React перерендерит область где state  
выводится.

`setState()` - асинхронная функция; может принимать значение или callback, возвращающий изменённое состояние;  
React оптимизирует обновление стейта, поэтому не гарантирует точность, для связанного состояние лучше useReducer.

```
// изменить состояние, основываясь на предыдущем значении
setState((prevState) => prevState + 1);
```

`initial` - начальное состояние; может быть вычисляемым значением.

```
// при такой передачи computeInitial не будет вычисляться при каждом изменении стейта
const [count, setCount] = useState(() => computedInitial());
```
___

### State в классовых компонентах

`this.state` - объект хранит все переменные состояния.

`this.setState(updater, cb)` - async функция обновления состояния (React не гарантирует мгновенное изменение state);  
можно вызывать в теле компонента; добавляет в очередь изменения в state, указывает React на необходимость ререндера;  
может принимать вторым аргументом callback, который выполнится когда state изменится; параметры: 
- `updater` - объект или функция (если новое состояние зависит от предыдущего), возвращающая объект `(state, props) => ({...})`
- `cb` - optional; вызовется после выполнения setState (но лучше использовать `componentDidUpdate()`)

```
class Example extends React.Component {
  state = {count: 0}  // так можно установить дефолтное значение

  constructor(props) {
    super(props);
    this.state = {count: 0};
  }

 updateCount = () => {
  const {count} = this.state;

  this.setState({count: count + 1});
 }

 // другой вариант обновления state
 updateCount = () => {
  this.setState((curState) => {count: curState.count + 1});  // так берётся актуальное значение
 }

 render() {
  return (
    <>
      <p>{this.state.count}</p>
      <button onClick={this.updateCount}>Add</button>
    </>
  );
 }
}
```

Если в state несколько свойств, для обновления одного не нужно передавать весь объект как в хуках.

Пример асинхронности setState:

```
updateCount = () => {
  this.setState({count: 1});

  console.log(this.state);  // {count: 0}
}
```

Чтобы код выполнился после обновления state, его нужно передать в setState вторым аргументом в коллбеке:

```
updateCount = () => {
  this.setState(
    {
      count: 2,
      name: 'Anna'
    },
    () => console.log(this.state)
  );
}
```

Если state обновлять не через сеттеры, то не будет реактивного обновления ui.
___

### useReducer

`useReducer(reducer, initial)` - альтернативный useState хук для более сложного связанного между собой state; синтаксис в стиле redux.

```
const initialState = {count: 0};
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      return {...state};
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <span>Count: {state.count}</span>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
};
```

Реализация переключателя:

```
const [value, toggleValue] = useReducer((prev) => !prev, true);

<button onClick={toggleValue}>Toggle</button>
```

Передача пропсов в reducer с помощью замыкания:

```
const reducer = (amount) => (state, action) => {
  switch (action) {
    case 'increment':
      return state + amount;
    case 'decrement':
      state - amount;
  }
};

const useCounterState = () => {
  const { data } = useQuery(['amount'], fetchAmount);

  return React.useReducer(reducer(data ?? 1), 0);
};

const App = () => {
  const [count, dispatch] = useCounterState();

  return (
    <>
      Count: {count}
      <button onClick={() => dispatch('increment')}>+</button>
      <button onClick={() => dispatch('decrement')}>-</button>
  );
};
```
___

### useContext

`useContext()` - хук для доступа к React.Context.
___
___

## Контролируемые формы

```
const Input = () => {
  const [value, setValue] = useState('');
  
  const handleChange = (event) => {
    setValue(event.target.value);
  };

  return (
    <>
      <input value={value} onChange={handleChange} />
      <span>{value}</span>
    </>
  );
};
```

То же в классовых компонентах:

```
class Input extends React.Component {
  state = {value: ''};

  // важно использовать стрелки, чтобы this = Input
  // у function this = undefined здесь, т.к. в компоненте strict mode, и пришлось бы привязывать контекст через bind
  handleChange = (event) => {
    this.setState({value: event.target.value});
  };

  render() {
    return (
      <>
        <input value={this.state.value} onChange={this.handleChange} />
        <span>{this.state.value}</span>
      </>
    );
  }
}
```

`SyntheticBaseEvent` - специальная обёртка React над нативным event браузера; используется внутри компонентов  
по дефолту вместо event.
___

## Props

Компоненты должны передавать пропсы только сверху вниз (Flux).


```
const App = () => {
  const handler = useCallback(() => console.log('click!'));

  return <Button handler={handler} />;
};

const Button = (props) => (
  <button onClick={props.handler}>Click</button>
);
```

**Поднятие state** - если 2 компонента должны иметь доступ к одной и той же переменной state, то её помещают  
в ближайшего общего родителя (но лучше в Context).
___

## Отображение списков

Для рендера списков используется `map`.

```
const List = () => {
  const [list, setList] = useState([
    'message 1',
    'message 2',
    'message 3'
  ]);

  return (
    <>
      <p>List</p>
      {list.map((message, idx) => <div key={`${message}${idx}`}>{message}</div>)}
    </>
  );
};
```

### key

`key` - специальный проп (должен быть уникальным), который React используется для определения изменившегося  
элемента; используется при рендере списков; влияет на производительность приложения.

Если при обновлении key не изменился, то компонент не будет перемонтирован. key должен быть уникальным, можно  
использовать индекс элемента в массиве, но только если не меняется порядок, иначе использовать id.

Даже при удалении элемента массива с key, React не перерендерит весь список. Но при изменении key у элемента,  
этот элемент перемонтируется.
___

## Fragment

`React.Fragment` - специальный компонент для группировки нескольких элементов; не добавляется в DOM; может принимать  
единственный проп - key (не в краткой форме).

```
<React.Fragment key={id}>
  <Comp1 />
  <Comp2 />
</React.Fragment>

// или
<>
  <Comp1 />
  <Comp2 />
</>
```

## Virtual DOM

**VirtualDOM** - оптимизированная копия DOM браузера; React создаёт при монтировании приложения; нужен для  
оптимизации рендера браузера.

Как работает:

```
      обновление компонента (new state/props)
                        v
             React строит новый VDOM
                        v
React сравнивает старый и новый DOM (reconciliation)
                        v
        ререндер обновлённых элементов DOM
```

### Reconciliation

**Реконциляция** - эвристический алгоритм React для сравнения двух деревьев; сложность O(n).

Сложность обычного алгоритма сравнения деревьев - O(n**3), т.е. для 100 элементов потребуется до 1 млн. операций.

Особенности реконциляции React:
- если меняется корневой элемент, то всё поддерево пересоздаётся
- элемент не пересоздаётся, если не изменился его key
- сложность O(n), т.е. для 100 элементов до 100 операций, что быстро
___

## props.children

`children` - специальный проп, который React создаёт, если в компонент вложен элемент.

Удобно для создания переиспользуемого компонента с разным контентом внутри. Также в children можно передать функцию.

```
function Card(props) {
  return (
    <div className={props.style}>
      {props.children}
    </din>
  );
}

// в классовых компонентах
this.props.children
```

**render props** - паттерн для контроля рендера дочернего компонента; родительский компонент передаёт дочернему  
функцию `render` (общепринятое имя), дочерний компонент вызывает `props.render` с нужным аргументом.
___
___

## React router

Библиотека для настройки маршрутизации внутри SPA.

[Doc](https://v5.reactrouter.com/web/guides/quick-start)

`npm i react-router-dom` - установка

`npm i @types/react-router-dom` - типизация

**4 вида роутеров:**
1. Browser router - использует Ordinary URL (через `/`)
2. Hash router - использует Hashed URL (через `#`); обычно используется для навигации по якорям внутри страницы; не  
   требует доп настройки сервера
3. Memory router - не использует адресную строку, хранит маршрут в памяти; используется для тестирования
4. Static router - не изменяет url; используется для SSR и в тестировании

### Компоненты роутера

`BrowserRouter` - обёртка для использования роутера, устанавливает тип роутера.

В `<BrowserRouter>` оборачиваем `<App />` в index.js
___

`Switch` - обёртка для всех роутов приложения; работает как switch-case - по порядку перебирает path переданных  
роутов до первого совпадения.
___

`Route` - обёртка для компонентов, которые нужно отобразить по заданному адресу; принимает:  
- `path` - url
- `exact` - требует точного совпадение url
- `component` - принимает компонент
- `element` - принимает reat element (по сути тот же component)
- `render` - принимает функцию рендера компонента; содержит параметры:
  - `history`
  - `location`
  - `match`

Компонент внутри Route отобразится при совпадении текущего url с path (частичное без exact). Все роуты обычно  
выносят в отдельный компонент Router.

```
<Route path="/profile">
  <Profile />  // 'profile123' тоже сработает
</Route>

<Route exact path="/home">
  <HomePage />
</Route>
```

Другие способы передачи компонента в Route:

```
<Route path="/" component={Comp} />

<Route
  path="/"
  render={(params) => {
    console.log(params);

    return <Home />;
  }}
/>

// пример необязательного параметра
<Route path="/chats/:chatId?" />
```

**Страница 404** - добавить в конце списка роутов, без указания path.
___

`Link` - обёртка над `<a>` для создания ссылки; принимает:  
- `to` - проп для адреса, относительно базового url.

```
<Link to="/">Home</Link>
```
___

`Redirect` - для редиректа на другую страницу; принимает:
- `to` - путь для ридеректа

```
if (!chat) {
  return <Redirect to="/chats" />
}
```
___

### Хуки роутера

`useParams()` - для доступа к квери-параметрам url, обеспечивает обновление компонента при их изменении.

```
// http://localhost:3000/chats/12

function Chats() {
  const { chatId } = useParams();  // '12'
  ...
}

// в классовых компонентах
this.params.match.params.chatId
```
___

`useRouteMatch()` - вернёт объект (match) со свойствами текущего роута (path, url); удобен при создании вложенных  
маршрутов.

```
function Chats() {
  const [chats, setChats] = useState(initialChats);
  const { path, url } = useRouteMatch();

  return (
    <Switch>
      <Route exact path={path}>
        <Comp1 />
      </Route>
      <Route path={`${path}/:chatId`}>
        <Comp2 />
      </Route>
    </Switch>
  );
}
```
___

`useHistory()` - вернёт объект history; используется для навигации (в v6 нет этого хука, см useNavigate).

history содержит методы:
- `go()` - перемещает по истории
- `goBack()` - то же что go(-1)
- `goForward()` - то же что go(1)
- `push(url)` - pushes url onto the history stack
- `block` - prevent navigation
- `replace()` - replace url on the history stack
- `location` - current location; лучше использовать location из пропа render компонента Route; may have:  
  - `pathname`
  - `search`
  - `hash`
  - `state`
- `length` - length of history stack
- `action` - current action (push, replace, etc)

```
const HomeButton = () => {
  const history = useHistory();

  const handleClick = () => {
    history.push('/home')
  };

  return (
    <button onClick={handleClick}>
      Home
    </button>
  );
};
```
___

`useLocation()` - returns the location object that represent the current url.
___
___

## HOC

**HOC** - (higher order component) компонент высшего порядка - функция которая принимает React компонент и возвращает  
новый компонент с каким-то сайд эффектом; по сути декоратор.

```
function Example(props) {
  return <span>{props.message}</span>
}

const withLogger = function(Component) {
  return (props) => {
    console.log(props);

    return <Component {...props} />;
  };
}

export ExampleWithLogger = withLogger(Example);
```

`connect` из Redux по сути HOC.
___
___

## Context API

**props drilling** - сквозная передача пропсов через промежуточные компоненты, которые не используют эти пропсы.

Props drilling нарушает важный принцип React - компонент должен получать только те данные, которые ему нужны.

**Context** - по сути переменная на высшем уровне приложения, доступ к которой можно получить из любого компонента.

**Как использовать:**
1. создать контекст через `createContext`
2. использовать поставщик контекста - обернуть дерево компонентов в context provider
3. передать необходимое значение в value атрибут провайдера
4. получить значение в любом компоненте дерева через `useContext`

```
export const UserContext = React.createContext();

export default function App() {
  return (
    <UserContext.Provider value="Read">
      <User />
    </UserContext.Provider>
  );
}

function User() {
  const value = React.useContext(UserContext);

  return <h1>{value}</h1>;
}
```

Контекст удобен для передачи/обновления общих данных компонентам на разном уровне вложенности (theme, language).  
Количество контекстов не ограничено; корневой компонент оборачивается во множество провайдеров контекста.  
Если компонент не обёрнут в Provider, то контекст будет иметь дефолтное значение в этом компоненте.

`createContext(initial)` - создаёт контекст; создаётся на верхнем уровне приложения (index.ts).

```
const ThemeContext = React.createContext({
  theme: 'dark'
});
```

`Context.Provider` - компонент-обёртка над корневым компонентом (или самом верхнем из тех, которые используют  
контекст); принимает атрибут `value` - значение, которое будет передано в компонент.

```
function App() {
  return (
    <ThemeContext.Provider value={{theme: 'dark'}}>
      <Router />
    </ThemeContext.Provider>
  );
}

// использование в компоненте
const contextValue = useContext(ThemeContext);
```

`Consumer` - компонент Context, внутри содержит render функцию, как children; устаревшее, лучше использовать useContext.

```
<ThemeContext.Consumer>
  {(value) => <div>{value}</div>}
</ThemeContext.Consumer>
```

**Context в классовом компоненте:**

```
// index.js
const MyContext = React.createContext({
  isLoggedIn: false
});

MyComp.contextType = MyContext;

ReactDOM.render(...);

// MyComp.jsx
class MyComp extends React.Component {
  render() {
    console.log(this.context);

    return ...;
  }
}
```

**Проблема производительности** - если провайдеру передать объект и обновлять в нём свойства, то это приведёт к  
ререндеру всех компонентов, использующих контекст.  
По этому не стоит использовать Context как полноценный state manager, он не подходит для часто  
обновляемых данных, подходит скорее как эквивалент глобальных переменных.

Себастьян Маркбейдж, по сути крестный отец Hooks и Context, говорит, что Context лучше всего подходит для  
низкочастотных, маловероятных обновлений и статических значений.
___
___

## State manager

- Redux
- MobX - использует observer паттерн
- Recoil - атомарный state, hooks API
- Hookstate - маленькая библиотека, использует хуки
___
___

## Работа с API

**API** - (application programming interface) описание методов взаимодействия между программами.

В контексте клиент-серверного взаимодействия, API это набор эндпоинтов.

**endpoint** - url, по которому клиент может передавать/получать данные с сервера.

```
const API_URL_GIST = 'https://api.github.com/gists/';

useEffect(() => {
  fetch(API_URL_GIST)
    .then((response) => response.json())
    .then((result) => setGists(result))
    .catch((error) => console.error(error));
}, []);
```

Сетевые операции часто подвержены ошибкам (медленное соединение, сервер недоступен, и т.д.), поэтому `.catch`  
блок обязательно.

Запрос обычно отправляется после первого рендера, первый рендер быстрее сетевого запроса. Это рекомендованный  
способ запроса на сервер из компонента.

```
useEffect(() => {
  request();
}, []);
```

В классовом компоненте:

```
componentDidMount() {
  fetch('https://...')
    .then((response) => response.json())
    .then((result) => {
      this.setState({
        isLoaded: true,
        data: result
      });
    })
    .catch((err) => {
      this.setState({
        isLoaded: false,
        error: true
      });
    });
}
```
___
___

## Мемоизация компонента

Позволяет избавиться от ненужного рендера.

### В class component

`shouldComponentUpdate(nextProps, nestState)` - позволяет описать условия ререндера; возвращает boolean.

```
shouldComponentUpdate(nextProps, nextState) {
  if (nextProps.text === this.props.text) {
    return false;
  }

  return true;
}

// или проще
shouldComponentUpdate(nextProps, NextState) {
  return nextProps.text !== this.props.text;
}
```

`React.PureComponent` - не перерендерится при получении одинаковых значений props, state; делает поверхностное  
сравнение (т.е. при изменении полей объекта в пропсах, не делает ререндер).

То есть `React.PureComponent` подходит если достаточно поверхностного сравнения пропсов и стейта (когда имеют  
примитивные значения). Для глубокого сравнения использовать `React.Component` + `shouldComponentUpdate`.

### В function component

`React.memo(Comp, skipRender(prevProps, nextProps))` - то же что React.PureComponent + shouldComponentUpdate в  
классовых компонентах; принимает мемоизируемый компонент и функцию сравнения; может мемоизировать function и class  
components.

Функция сравнения должна вернуть true - пропустить рендер, false - рендер (в shouldComponentUpdate ровно наоборот).

```
const ChildMemo = React.memo(Child, (prevProps, nextProps) => {
  return prevProps.text === nextProps.text;
});
```

`React.memo(Comp)` - без второго аргумента то же что PureComponent; т.е. не перерендерит при том же значении  
props/state при поверхностном значении.

**Зачем memo?**
```
const App = () => {
  const [count, setCount] = useState(0);

  return (
    <>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Click</button>
      <ExpensiveComponent />  // вот это лучше обернуть в memo, т.к. он будет ререндериться при каждом
    </>                       // изменении count
  );
};
```

|  component  |      shallow equal       |                    deep equal                |
|-------------|--------------------------|----------------------------------------------|
|    class    |   React.PureComponent    | React.PureComponent + shouldComponentUpdate()|
|  function   |    React.memo(Comp)      |          React.memo(Comp, skipRender)        |
___
___

## React Profiler

Инструмент react devtools для контроля рендера и оптимизации (типа lighthouse).
___
___

## PWA

**Progressive web application** - приложение может частично работать без интернета, как десктопное/мобильное.

Использует обёртку браузера без элементов управления. Может работать без инэта - использует push через webWorker.  
Примерно то же делает Electron.  

Pwa не работает в ios, safari запрещает pwa

`manifest.json` в cra - это для pwa.
___
___

## Class component

### render

`render()` - при вызове проверяет `this.props/this.state`; возвращает:
- react element (jsx разметка)
- массивы и фрагменты
- порталы
- строки и числа (text node)
- boolean, null (ничего не рендерит)

`render()` должна быть чистой функцией.  
Не вызывается если `shouldComponentUpdate()` вернёт false.

### constructor

`constructor()` - вызывается до mount; используется для:
- инициализации state
- bind обработчиков к экземпляру

`constructor()` можно не использовать, если не определяем state и не биндим методы.  
В конструкторе сначала нужно вызывать `super(props)`, иначе `this.props` будет undefined.

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {}

  render() {}
}
```

### getDerivedStateFromProps

`getDerivedStateFromProps(props, state)` - (derived - производное) позволяет изменять state в ответ на изменение  
пропсов без дополнительного рендера; возвращает объект для обновления state или null; вызывается перед каждым  
рендером.

Метод существует для тех редких случаев, когда state зависит от изменений в пропсах.

```
static getDerivedStateFromProps(props, state) {
  if (props.userId !== state.prevPropsUserId) {
    return {
      prevPropsUserId: props.userId,
      email: props.defaultEmail
    };
  }

  return null;
}
```
___

### getSnapshotBeforeUpdate

`getSnapshotBeforeUpdate(prevProps, prevState)` - позволяет получать инфо из DOM перед изменением (например  
положение scroll); вызывается перед обновлением DOM, после render(); значение возвращённое этим методом передаётся  
как параметр `snapshot` в `componentDidUpdate()`.

```
class ScrollingList extend React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;

      return list.scrollHeight - list.scrollTop;  // запоминаем значение, чтобы использовать позже
    }

    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot !== null) {
      const list = this.listRef.current;

      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  render() {
    return <div ref={this.listRef}></div>;
  }
}
```
___

### ref

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.ref = React.createRef();
  }

  render() {
    return (
      <div ref={this.ref}></div>
    );
  }
}

console.log(this.ref.current);
```
___

### Error boundary

**Error boundary** - (предохранитель) компонент React, который отлавливает ошибки js в дереве дочерних компонентов,  
сохраняет их в логе и выводит запасной ui; ошибки не отловленные предохранителем ведут к размонтированию приложения.

Не отловит ошибку, если она:
- в обработчике событий
- в async коде
- server-side rendering
- в самом предохранителе

Классовый компонент является предохранителем, если имеет хотя бы один из методов:
- `getDerivedStateFromError()` - для рендера запасного ui
- `componentDidCatch()` - для логирования ошибок

`getDerivedStateFromError(error)` - static; вызовется после возникновении ошибки у компонента потомка; возвращает  
значение для обновления state; сработает во время рендера - нельзя использовать side effects.

`componentDidCatch(error, info)` - вызывается после возникновения ошибки; используется для логирования ошибок;  
вызывается во время фиксации - можно использовать side effects; обработка ошибок отличается в разных средах:
- develop - ошибки всплывают до window (можно перехватить через window.onerror, window.addEventListener('error'), cb)
- production - ошибки не всплывают

- `error` - перехваченная ошибка
- `info` - объект с полем `componentStack`

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = {hasError: false};
  }

  static getDerivedStateFromError(error) {
    return {hasError: true};  // обновить состояние, чтобы при следующем рендере показать запасной ui
  }

  componentDidCatch(error, info) {
    sendLogSomewhere(error, info);
  }

  // обработка ошибок в хендлере (не ловится предохранителем)
  handleClick() {
    try {
      ...
    } catch (err) {
      this.setState({error});
    }
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong!</h1>;
    }

    return this.props.children;
  }
}
```
___

### forceUpdate

`forceUpdate()` - вызывает рендер, пропуская shouldComponentUpdate; используется редко и не особо приветствуется;  
вызывается непосредственно из тела компонента (как setState).

```
rerender() {
  this.forceUpdate();
}

render() {
  return <input type="button" onClick={this.render} />;
}
```
___

### defaultProps

`defaultProps` - свойство классового компонента для установки дефолтных пропсов; сработает если пропсы не переданы  
или undefined (null - валидный проп).

```
Example.defaultProps = {
  color: 'blue'
};
```
___
___
