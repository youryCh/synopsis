**Redux** - state manager.

**позволяет реализовать:**
- SST - single source of truth; все компоненты, зависящие от данных, получают их из единого источника
- immutable - при обновлении state, не изменяем его, а создаём новый (через spread)
- Flux - однонаправленный поток данных; изменяем state только через dispatch из компонента, не напрямую

**store** - специальный объект, который хранит данные и методы для работы с ними (dispatch, getState).

**state** - данные в store.

`dispatch(action)` - функция для изменения данных; принимает action.

`action()` - возвращает объект с полями `type` и `payload`; с помощью type, reducer определяет какие изменения внести  
в store; `type` должны быть уникальными (обычно хранят в строковой константе, имя в upper case).

`reducer(state, action)` - чистая функция, которая описывает какие произвести изменения в store в ответ на каждый из action, возвращает обновлённый store, либо текущее состояние store; аргументами принимает текущий state и action.

**Принцип работы:**
1. данные хранятся в store
2. для изменения store вызвать dispatch, передав в него action (задиспатчить экшен).
3. Redux вызывает reducer, передав туда актуальный store и задиспатченный экшен.
4. reducer изменяет данные и возвращает новое состояние store.

`npm i redux`  
`npm i react-redux` - сама библиотека и вспомогательная библиотека для React.

`createStore(reducer)` - создание стора; принимает функцию reducer.

`combineReducers()` - для передачи нескольких редюсеров в виде объекта.

```
import { createStore, combineReducers } from 'redux';

const rootReducer = combineReducers({
  profile: profileReducer
});
```

Пример action:

```
// actions/profile.js

export const CHANGE_NAME = 'PROFILE::CHANGE_NAME';  // синтаксис по договорённости

export const changeName = (name) => ({
  type: CHANGE_NAME,
  payload: { name }
});
```

`'PROFILE::CHANGE_NAME'` - имя action.type обычно начинается с названия reducer, для уникальности и проще отслеживать  
в devtools.

Пример reducer:

```
// reducers/profile.js

import { CHANGE_NAME } from './.';

const initialState = {
  name: 'Anna',
  age: 39
};

export default function reducer(state = initialState, action) {
  switch (action.type) {
    case CHANGE_NAME:
      return {
        ...state,
        name: action.payload.name
      };

    default:  // обязательно
      return state;
  }
}
```

Подключение store:

```
// index.js

import { Provider } from 'react-redux';
import { store } from './store.js';

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <Router />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

Пример создания store:
```
// store.js

const rootReducer = combineReducers({
  profile: profileReducer
});

export const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
```
___

### Action creator

`actionCreator()` - функция, которая возвращает объект экшена для диспатча (диспатч принимает объектный литерал);  
может содержать логику для проверки/обработки payload до отправки в dispatch; обычно называют как экшен, но в нижнем  
регистре.

```
const ADD_TODO = 'ADD_TODO';

const addTodo = (text) => {
  // здесь может быть логика валидации, фильтрации, ...

  return {
    type: ADD_TODO,
    payload: { text },
  };
};

dispatch(addTodo(text));
```

Фабрика action creator:

```
const makeActionCreator = (type, keys) => {
  return (...values) => {
    const payload = {};

    keys.forEach((_, index) => {             // лучше reduce
      payload[keys[index]] = values[index];
    });

    return { type, payload };
  };
};

const addTodo = makeActionCreator(
  ADD_TODO,
  ['text'],
);
```

**Готовые библиотеки** action creators:
- redux-actions
- redux-act
___

## React-redux

**react-redux** - библиотека для подключения и работы с стором redux, предоставляет хуки.

`npm i -D react-redux`

`Provider` - подключает стор.

```
export function App() {
  return (
    <Provider store={store}>
      <Router />
    </Provider>
  );
};
```

`useSelector(selector, compare)` - хук для получения данных store в компоненте; принимает:  
- `selector()` - функция принимает state, возвращает нужную часть state
- `compare()` - опциональная функция сравнения (смотри 160)

```
const { name } = useSelector((state) => state.profile);
```

`useDispatch()` - возвращает функцию dispatch(), для изменения store.

```
const dispatch = useDispatch();

const handle = () => {
  dispatch(changeName(inputValue));
};
```

|  Class component  |  Function component  |
|  ---------------  |  ------------------  |
|       Redux       |  Redux + react-redux |
|  store.getState() |    useSelector()     |
|  store.dispatch() |    useDispatch()     |
___

### Оптимизация useSelector

`useSelector` - отвечает за обновление компонента при изменении store. По дефолту использует ссылочное сравнение  
prev === next, что может приводить к лишним рендерам. Можно передать функцию сравнения вторым аргументом.

```
const chats = useSelector(
  (state) => state.chats,
  (prev, next) => prev.length === next.length
);
```

`shallowEqual` - функция react-redux для поверхностного сравнения.

```
import { shallowEqual } from 'react-redux';

const chats = useSelector(
  (state) => state.chats,
  shallowEqual
);
```

При передаче стрелки в useSelector, вызов не может кешироваться, поэтому функцию селектор лучше выносить в  
отдельную функцию (и отдельный файл):

```
// chats/selectors.js
export const getChats = (state) => state.chats;

// в компоненте
const chats = useSelector(getChats);
```

### Reselect

Библиотека для создания сложных мемоизированных селекторов, т.е. не будет лишних ререндеров.

`createSelector()` - создаёт мемоизированные селекторы; принимает массив селекторов и трансформирующую функцию.

```
const selectA = (state) => state.a;
const selectB = (state) => state.b;
const selectAB = createSelector(
  [selectA, selectB],
  (a, b) => a + b
);
```
___
___

## Redux в классовых компонентах

`connect(mapStateToProps, mapDispatchToProps)` - функция Redux для подключения компонента к store; это HOC, который  
оборачивает компонент и подмешивает в пропсы часть стейта и задиспатченные экшены.

```
connect(
  mapStateToProps,  // если state не нужен, передать null
  mapDispatchToProps
)(App);
```

`mapStateToProps()` - принимает state; передаёт нужную часть state в пропсы компонента; типа useSelector.

```
const mapStateToProps = (state) => {
  return state.profile
};
```

Пример функции-генератора mapStateToProps:

```
// принимает строку с названием компонента, возвращает mapStateToProps
const mapStateToPropsGenerator = (component) => {
  switch (component) {
    case 'Comp1':
      return (store) => ({  // по сути это mapStateToProps
        user: store.user
      });
    case 'Comp2':
      return (store) => ({
        page: store.page
      });
    default: 
      return undefined;
  }
};
```

`mapDispatchToProps()` - передаёт задиспатченные экшены (методы для обновления нужного поля store) в пропсы  
компонента; т.е. переданный задиспатченный экшен будет доступен через `this.props`

```
mapDispatchToProps = (dispatch) => ({
  changeName: (newName) => dispatch(changeName(newName))
});
```
___
___

## Redux middleware

**Middleware** - посредник (прослойка) между action и reducer для сайд эффектов, обычно для async action (fetch).

```
const middleware = (store) => (next) => (action) => {
  console.log('Some side effect');
  setTimeout(() => {...}, 1000);

  return next(action);
};
```

Без middleware Redux работает только синхронно.

Библиотеки middleware:
- redux-thunk
- redux-saga (смотри `./redux-saga.md`)
- redux-observable

`applyMiddleware()` - функция Redux для добавления middleware.

### Redux-thunk

`npm i -D redux-thunk` - установка

Подключение:

```
// store/store.js
import {
  applyMiddleware,
  combineReducers,
  createStore,
  compose
} from 'redux';
import thunk from 'redux-thunk';

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE || compose;

export const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(thunk))
);
```

### Redux-logger

Библиотека для логирования в Redux.

`npm i -D redux-logger` - installation.

```
const store = createStore(
  rootReducer,
  applyMiddleware(logger)
);
```

### Redux-saga-routines

Библиотека action creator для Redux.

`yarn add redux-saga-routines`

**Routine** - action creator с 5 стандартными типами экшенов, не нужно для каждого запроса писать экшены для  
TRIGGER -> REQUEST -> SUCCESS/FAILURE -> FULFILL.
___

## Redux-persist

Библиотека позволяет сохранять store между сессиями на стороне клинте (localStorage по дефолту).

`npm i -D redux-persist` - installation.

**Как работает:**

При размонтировании приложения persist сохраняет данные redux store в постоянном хранилище. При последующем  
запуске приложения, извлекает данные из хранилища и помещает в store.

Создание:

```
// store.js
import { persistReducer, persistStore } from 'redux-persist';
import storage from 'redux-persist/lib/storage';

const persistConfig = {
  key: 'root',  // ключ по которому будут храниться данные в хранилище
  storage  // localStorage
};

// это в store вместо rootReducer
const persistedReducer = persistReducer(
  persistConfig,
  rootReducer
);

export const persistor = persistStore(store);
```

Подключение:

```
// src/index.js
import { PersistGate } from 'redux-persist/integration/react';

ReactDOM.render(
  <Provider store={store}>
    <PersistGate persistor={persistor} loading={<p>Loading...</p>}>
      <App />
    </PersistGate>
  </Provider>,
  document.getElementById('root')
);
```
___
___
