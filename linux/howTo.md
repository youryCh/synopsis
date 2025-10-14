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

## Найти wsl ubuntu файлы в винде

`C:/Users/your_user/AppData/Local`
___

## Добавить SSH ключь в WSL

Существующие ключи валяются в Users/<yser>/.ssh .
В WSL в ~ директории надо создать такую же структуру: директорию .ssh (просмотр скрытых директорий: ls -la), id_rsa, id_rsa.pub, known_hosts.

И перетащить содержимое соответствующих файлов типа так:
- vim id_rsa
- ctrl + v
- :wq (save & quit)
- и так все три файла

Затем дать права доступа:
- chmod 600 /home/<user>/.ssh/id_rsa
- chmod 700 /home/<user>/.ssh
___

## Install node.js to WSL

- `sudo apt update && sudo apt upgrade -y` - update packages
- `sudo apt install -y nodejs npm` - install node.js (12 version)

Для установки последней стабильной версии придётся поставить nvm:
- `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash` - install nwm
- reload terminal
- `nvm install node` - install lts node
___

## Install nvm

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash`
___
