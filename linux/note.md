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

`kill <PID>` - завершить процесс с конкретный id.

`killall` - завершить все процессы.

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

`dig` - DNS lookup utility; позволяет достать ip.

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


