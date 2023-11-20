# Redux Tool Kit

При установке rtk, redux отдельно ставить не надо.

`createReducer()` - создать reducer.

`createAction()` - создать actionCreator.

```
const increment = createAction('INCREMENT');

export default createReducer(initialState, {
  [increment]: function(state) {
    state.count = state.count + 1  // rtk изменяет нужное поле и не трогает остальной state
  }                                // т.е. это только с виду мутация
});
```
___

## Slice

По сути тот же reducer, но в другой обёртке.

```
const slice = createSlice({
  name: 'slice',  // имя слайса
  initialState: {...},
  reducers: {  // экшены
    increment(state) {
      state.count = state.count + 1
    },
    addTodo(state, action) {
      state.todos.push(action.payload)
    }
  }
});

// пример экспорта
export default slice.reducer;
export const { increment, addTodo } = slice.actions;
```
___
