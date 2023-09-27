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

