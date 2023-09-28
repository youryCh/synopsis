## Notification API

> Апи для работы с браузерными всплывающими уведомлениями;  
> работает оч странно.

`Notification.requestPermission()` - запросить у пользователя разрешение на показ уведомлений.

`new Notification('title', { params })` - создать инстанс уведомлений; вызвать уведомление.

`Notification.close()` - закрывает инстанс Notification.

```
const onShowNotification = () => new Notification(...);

const onClose = () => Notification.close();
```

___

##  Navigator

> **navigator** - интерфейс доступа для свойств и методов user agent.

```
navigator.isOnline;

navigator.vibrate();
```

`isInputPending()` - вернёт true если пользователь пытается взаимодействовать со страницей.

```
navigator.scheduling.isInputPending();
```

___

## Scheduler API

`window.scheduler` - Механизм браузера для планирования задач в main thread.

`postTask(task, [options])` - позволяет планировать задачи, разбивать long task.
  - `task` - функция для планирования
  - `options:`  
    - `priority:`  
      - `background` - низкий приоритет
      - `user-visible` - средний, дефолтный
      - `user-blocking` - высокий, для критических таск

```
scheduler.postTask(showSpinner, {
  priority: 'user-blocking'
});
```

___

`windows.postMessage(message, targetOrigin)` - метод для кроссдоменной отправки сообщений (между разными окнами браузера).
  - `message` - данные, сериализуются автоматически
  - `targetOrigin` - origin получателя

> Работает через события.  
> Целевое окно слушает событие `message`.

```
otherWindow.postMessage(message, targetOrigin);

// otherWindow - ссылка на другое окно (из window.open например)
```

___

`window.requestIdleCallback()` - переданная функция будет вызываться во время простоя браузера.
  - `options:`  
    - `timeout` - ограничение максимального времени выполнения

```
const handle = window.requestIdleCallback(
  callback,
  options
);  // возвращает id, который можно использовать для отмены коллбеков
```

`window.cancelIdleCallback(id)` - для отмены коллбеков.

___

`crypto` - [doc](https://developer.mozilla.org/ru/docs/Web/API/Crypto) веб-интерфейс; предоставляет базовые криптографические функции.

```
window.crypto.randomUUID();

window.crypto.getRandomValues(arr);  // сделает массив криптографически случайных чисел

// и много других методов
```

___


