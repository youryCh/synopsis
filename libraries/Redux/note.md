## Action creator

> Функция которая возвращает объект экшена для диспатча (диспатч принимает объектный литерал).

> Action creator может содержать логику для проверки/обработки payload до отправки в dispatch.

> Action creator обычно называют как экшен, но в нижнем регистре.

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


// фабрика action creator
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

Готовые библиотеки action creators:
- redux-actions
- redux-act

___


