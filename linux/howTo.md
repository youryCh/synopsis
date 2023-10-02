## Получить system info

`uname -a` - system info.

`cat /etc/*rel*` - extended system info.

___

## Get public IP

`curl ifconfig.me ; echo`

`curl ifconfig.co ; echo` - same

___

## Создать много файлов/директорий

```
// создать 100 файлов вида file1.txt
touch file{1..100}.txt

// удалить 5 файлов
rm file{1..5}.txt

// создать 4 директории
mkdir {images,css,src,scripts}  // без пробелов

// создать app.html, app.css, app.js
touch app.{html,css,js}
```

___


