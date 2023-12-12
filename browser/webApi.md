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

`navigator` - интерфейс доступа для свойств и методов user agent; содержит:  
- `online` - подключен ли интернет
- `vibrate()`
- `scheduling:`
  - `isInputPending()` - взаимодействует ли пользователь со страницей сейчас
- `clipboard:` - для работы с буфером
  - `write()` - для сохранения данных в буфер; универсальный; асинхронный (вернёт Promise)
  - `writeText()` - только для текста; асинхронный
  - `read()` - чтение из буфера (браузер покажет модалку с запросом согласия на чтение)
  - `readText()`
- `permissions:` - для работы с разрешениями юзера (показывает модалки браузера)
  - `query()` - покажет модалку с запросом разрешения юзера; вернёт Promise
- `geolocation:`
  - `getCurrentPosition()` - браузер кинет окно запроса разрешения; вернёт объект геолокации
  - `watchPosition()` - вернёт callback, который вызовется при смене позиции
  - `clearWatch()` - удаляет коллбек установленный через watchPosition

```
navigator.online;  // также есть события window 'onLine', 'offLine'
navigator.vibrate();
navigator.scheduling.isInputPending();
window.navigator.permissions.query({
  name: 'clipboard-read'  // или clipboard-write
});
```
___

## Scheduler API

`window.scheduler` - механизм браузера для планирования задач в main thread.

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

## Crypto

`crypto` - [doc](https://developer.mozilla.org/ru/docs/Web/API/Crypto) веб-интерфейс; предоставляет базовые криптографические функции.

```
window.crypto.randomUUID();

window.crypto.getRandomValues(arr);  // сделает массив криптографически случайных чисел

// и много других методов
```
___

## Audio API usage example

```
function beep() {
  const audioContext = new window.AudioContext();

  const oscillator = audioContext.createOscillator();
  const gainNode = audioContext.createGain();

  oscillator.connect(gainNode);
  gainNode.connect(audioContext.destination);
  oscillator.start(audioContext.currentTime);
  oscillator.stop(audioContext.currentTime + 0.4);

  // const context = new window.AudioContext();

  // const osc = new OscillatorNode(context);
  // const gain = context.createGain();

  // osc.frequency.setValueAtTime(261.6, 0);
  // gain.gain.value = 0.5;

  // osc.connect(gain).connect(context.destination);

  // osc.start();
}

useEffect(() => beep(), []);
```
___
