`clear` | `CTRL+L` - очистить терминал.

`exit` | `CTRL+D` - logout.

`CTRL+R` - поиск команды в истории по введённым символам.

`vi <file>` - open file in Vi editor.

___

`CTRL+INSERT` - copy in bash.
`SHIFT+INSERT` - paste in bash.

___

`diff` - сравнить файлы.
  - `-bur` - подробности

```
// выведет диф в файл
diff dir1/file2 dir2/file2 > change.diff
```

___

`PS1` - (Prompt statement) переменная окружения, определяет как будет выглядеть приглашение shell (то что до $); находится в ~/.bashrc.

___

## Processes

`pgrep` - найти процесс по имени/другим атрибутам; вернёт pid.

`kill <PID>` - завершить процесс с конкретный id.
  - `-15` - SIGTERM

`killall` - завершить все процессы.

`pkill` - найти процесс по имени/атрибуту и завершить.
  - `-f <string>` - найти по шаблону (string)

___

`cal` - calendar.

___

`w` - show who is logged.

`users` - список зарегистрированных юзеров.

`last` - список юзеров входивших в систему.

`finger <user>` - user info.

___

`uname` - OS info.
  - `-a` - расширенная инфа
  - `-vr` - номер ОС, версия ядра

___

`asdf` - runtime version manager tool.

___

`printf` - format and print date.

`base64` - encode/decode data and print.

```
echo 'hi' | base64

printf 'user:pass' | base64
```

___

## Exit code

> Код завершения программы.

- `0` - success
- `1` - (и более) error

`echo $?` - exit code последней запущенной команды.

___

`jq` - cli JSON processor/formatter.

```
// выведет в читаемом виде
jq . products.json
```

___

`dig` - DNS lookup utility; позволяет достать ip, все доступные dns записи.

**TTL** - time to live (sec).

___

## Basic calculator

`bc` - калькулятор устанавливается отдельно.

`sudo apt install bc`

```
> bc
> scale=2; 3/2  // scale - количество знаков после запятой
> 1,50
```

___

## Sed

`sed` - stream editor; потоковый редактор текста; позволяет редактировать без открывания файла в редакторе;  
обычно используется для вставки/замены; поддерживает regexp.
  - `-i` - изменить файл; без него меняет только вывод команды

`'s/что_заменить/на_что/'` - regexp; по дефолту (без `g`) заменит первое вхождение  
  - `g` - замена всех вхождений
  - `<n>` - замена n вхождений
  - `<n>g` - замена всех вхождений, начиная с n

```
// замена по заданному шаблону
sed 's/unix/linux/g' file -i

// замена в заданной строке
sed '3 s/unix/linux/' file
```

___

`nohup` - запустить процесс, который не завершится с выходом из терминала.
  - `&` - запуск в фоновом режиме

___

`time <command>` - выведет время выполнения любой команды.

___

## Curl

`curl` - client URL; утилита для сложных запросов.
  - `--head/-I` - не получать тело ответа (по сути HEAD)
  - `-X/--request <method>` - указание метода запроса
  - `-H/--header <headers>` - указание заголовков
  - `-v/--verbose` - подробный вывод запроса/ответа
  - `-d/--data` - для отправки данных в body
  - `-O` - для скачивания файла
  - `-k/--insecure` - игнорировать сертификаты
  - `-F` - загрузка файлов
  - `-u/--user <user:pass>` - аутентификация

`apt install curl` - установка, если не идёт дефолтом с дистрибутивом.

### GET

```
// GET запрос; вернёт html
curl https://ya.ru

// то же, но с указанием порта
curl ya.ru:443

// перенаправление ответа в файл
curl https://ya.ru > ya.html

// то же, но если ya.html не существует, то создаст его
curl https://ya.ru | tee ya.html

// GET to localhost
curl -H 'Content-Type: application/json' localhost:3000/users
```

### HEAD

> HEAD запрос возвращает такие же заголовки как в GET запросе (но это по спецификации, на практике не всегда).

```
// HEAD запрос
curl --head https://ya.ru

// или
curl -I https://ya.ru
```

```
// для точного получения GET заголовков, в отличии от неточного HEAD
// --head здесь для игнорирования body
// -X GET задаёт метод запроса
curl --head -X GET https://ya.ru
```

### POST

```
// POST запрос с телом
curl -X POST https://ya.ru\               // \+enter - экранирование перевода строки для многострочного ввода
  -H "Content-Type: application/json"\
  -d '{"name": "Anna"}'

// 2 варианта отправить данные в теле: 
// (1) строка key=value
curl --data "param1=value&param2=value" https://ya.ru

// (2) json
curl -H 'Content-Type: application/json' --data \
  {"param1": 123}
```

### Download

```
// скачать файл
// можно скачать несколько файлов одной командой
curl -O https://ya.ru    // скачает под оригинальным именем

// скачать и сохранить в file
curl -O file https://ya.ru
```

`-F` - загрузка файлов.

### Authentication

```
curl -u <user:pass>
```

___

`df` - покажет использование диска файлами.
  - `-h` - human readable size

___

`du` - оценка места, занимаемого файлом на диске.

___

## Администрирование

```
// примет юзера с root-правами для команд cat и df
user ALL=(ALL) NOPASSWD:/usr/bin/cat,/usr/bin/dt
```

> **Процесс** - единица работы в linux; представление запущенных программ в ОС (например в браузере 1 вкладка - 1 процесс).

> Процесс внутри себя может делиться на потоки, для производительности или параллельного запуска.

> **Сигнал** - позволяет управлять процессом.

> Например `kill -15 606827` отправляет процессу 606827 сигнал 15 - SIGTERM - завершить процесс.  
> Запущенные процессы слушают и обрабатывают сигналы.

**SIGTERM** - не гарантирует завершение процесса.

**SIGKILL** - сигнал принудительного завершения процесса; выполняется ОС, а не процессором.

> **Supervisor** - процесс который контролирует другие процессы (запуск, перезапуск, остановка); запускается первым  
> в системе.

### Systemd

> Systemd позволяет гибко настроить лимиты ресурсов памяти и прочее; собирает все логи запущенных процессов.

`systemd` - программа-супервизор; есть в большинстве дистрибутивов linux.

`nginx.service` - файл описывающий как запускать процесс через systemd; хранится в /lib/systemd/system/*

`journalctl` - показать systemd логи

___

**Сетевой интерфейс** - программный способ взаимодействия linux с сетевой картой.

**Виртуальный сетевой интерфейс** - не связан с железом, существует на уровне ОС (например localhost).

`itconfig` - просмотр/настройка сетевых интерфейсов внутри ОС; устанавливается отдельно.

Показывает:  
- `eth0` - (ethernet0); интерфейс связанный с сетевой картой (Ethernet)
- `lo` - (loopback device); виртуальный дефолтный интерфейс linux; используется для отладки сетевых программ  
и запуска серверных приложений на локальной машине;  
ip-адрес постоянный 127.0.0.1, dns-имя localhost.

> Большинство сервисов стартует на localhost, для безопасности (нельзя достучаться снаружи).  
> Сервис, запущенный на lo, недоступен на том же порте в eth0

```
// пример запуска дефолтного php-сервера
php -S localhost:8000
```

`0.0.0.0` - псевдоиндекс; связывает запускаемый сервис со всеми доступными интерфейсами системы (и localhost и снаружи).

___

`awk` - утилита/скриптовый язык для работы с текстом в cli.

```
// выведет только вторую колонку
df -h | awk '{print $2}'
```

___

`ping <url>` - утилита для отправки echo-request; можно добыть ip-адрес.
  - `-c <n>` - количество обращений
  - `CTRL+C` - quit

___

## Telnet

`telnet <ip> <port>` - утилита для http-запросов по telnet протоколу (не работает с https).

```
telnet 74.125.21.139 80

// 80 - дефолтный http-порт
// если используется другой порт, то указать так: http://site.com:123
```

> Telnet сначала устанавливает TCP соединение, затем приглашает к взаимодействию по HTTP;  
> если не вводить запрос, соединение закроется сервером секунд через 30.  
> Enter - отправить запрос.

```
    HEAD / HTTP/1.0

method|path|protocol

HEAD / HTTP/1.0       // request line
User-Agent: chrome    // headers
```

> Заголовки регистронезависимы, порядок не важен;  
> перевод строки - новый заголовок;  
> 2 перевода строки - ввод завершён.

```
// запрос к localhost
telnet localhost 8080
GET / HTTP/1.1
Host: myHost.local
```

> Body запроса отделяется пустой строкой.

```
// отправка формы (не json-формат)
telnet localhost 3000

POST /session/new HTTP/1.1
Host myHost.local
Content-type: application/x-www-form-urlencoded
Content-Length: 30

username=Yuri&password=secret
```

```
// пример PATCH - частичное обновление данных юзера с id = 1
PATCH /users/1 HTTP/1.1
Host: site.com
Content-Length: 21
Content-Type: application/json

{"name": "Anna"}
```
___


