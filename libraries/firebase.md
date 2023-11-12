**Firebase** - библиотека от Google; предоставляет сервисы для авторизации, аналитики, noSQL db, и др.

`PrivateRoute/PublicRoute` - скрытие маршрутов в зависимости от аутентификации.

`firebase.auth()` - методы аутентификации:
- `createUserWithEmailAndPassword(email, pass)` - метод создания юзера
- `signInWithEmailAndPassword()` - для логирования
- `onAuthStateChanged(cb)` - загрузка текущего залогиненного юзера
- `signOut()` - разлогинить

```
try {
  await firebase.auth().createUserWithEmailAndPassword(email, pass);
} catch (err) {
  setError(err);
}
```
___

### Realtime DB

Работа rdb основана на webSocket. База сама уведомляет приложение о своих изменениях.

```
// подключение
const db = firebase.database();

// добавление сообщения в db
db.ref('messages').child(chatId).push(message);

// изменение/добавление если нету
db.ref('message').child(chatId).child(message.id).set(message);
```
___
