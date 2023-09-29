> **Redux-saga** - это менеджер сайд-эффектов (middlewares).

> **Сайд-эффект** - (middleware) это действие которое выполняется на диспатч экшена.

> **Saga** - паттерн управления процессами, которые выполняются транзакционно, т.е. как одно атомарное действие.

`yarn add redux-saga` - установка

`createSagaMiddleware()` - функция создания мидлвар; с помощью неё создаётся инстанс и передаётся в  
compose -> applyMiddleware -> createStore редакса (или подобную в RTK).

```
const sagaMiddleware = createSagaMiddleware();

const store = createStore(
  ...
  compose(applyMiddleware(sagaMiddleware))
);

sagaMiddleware.run(saga);  // watcher
```

### Sagas

**2 вида:**
- watcher-saga - слушает actions и назначает выполнение worker-saga; в имени обычно используется saga.
- worker-saga - выполняет делегируемое watcher-saga

> Каждая saga это generator.

```
// пример worker-saga; запускает fetchPosts на каждый диспатч экшена POST_REQUESTED
export function* sags() {
  yield takeEvery(POST_REQUESTED, fetchPost);
}
```

### Effects

> Это помощники для прослушивания экшенов; импортируются из `redux-saga/effects`.

`TakeEvery` - хелпер который прослушивает action (конкретный или '*') и запускает worker-saga.

`put()` - эффект для диспатча внутри саги.

`call()` - запускает функцию.

`takeLatest` - выполняет только одну текущую задачу, игнорируя остальные.

**Flow:**
- диспатчим экшен в компоненте
- saga-watcher прослушивает экшен с таким типом и запускает saga-worker
- saga-worker делает запрос на сервер

### Channels

> Позволяют создавать очередь из последовательных запросов;  
> очередь хранится в буфере.

**3 вида каналов:**
- channel - используется для общения саг между собой
- action channel - привязать экшены
- event channel - позволяет привязать внешний источник данных (WebSockets)

> В отличии от `takeEvery + put` (выполняет запросы конкурентно), action channel выполняет запросы последовательно.

### Blocking/non-blocking effects

> **Blocking** - когда saga делает yield эффекта, будет ожидать завершения вызова, затем перейдёт к выполнению  
> следующего эффекта.

> Блокирующие: call, take, retry, takeMaybe

> Не блокирующие: put, fork, takeEvery, takeLatest

`all` - блокируется если в массиве есть блокирующий эффект, иначе - не блокирующий.

`fork` - attach effect (ошибки в нём всплывают до родителя); не блокирующий аналог call; возвращает task.

`spawn` - как fork, но detached effect - ошибки не всплывают.

> При использовании spawn, если произойдёт ошибка, то выполнение родительской саги продолжится.  
> Также, если отменить родительскую сагу, то задачи spawn не отменятся, нужно отменять явно каждую.  
> Т.е. fork привязан к родителю, а spawn нет.

`cancel` - эффект отменяет задачу.

`cancelled` - эффект вернёт true если сработал cancel.


