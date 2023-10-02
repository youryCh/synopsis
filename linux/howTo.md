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

## Add new user

`adduser  <user>` - add user.

`usermod -aG sudo <user>` - add user in sudo group.

`su - <user>` - login as user.

___

## Сменить дефолтный редактор

> В Ubuntu по дефолту используется nano.

`sudo update-alternatives --config editor` и выбрать например vim.

___

## Завершить процесс принудительно

(1)
```
// вернёт pid процесса python, например 22
pgrep python

// завершить через SIGTERM
kill -15 22
```

(2)
```
// найти и завершить процесс
pkill python

// или найти по шаблону и завершить
pkill -f 'http.server'
```
___

## Посмотреть объём systemd логов

`du -h --max-depth=1 /var/log/journal`

___


