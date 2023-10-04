> **Python** - интерпретируемый язык с сильной динамической типизацией.

> **CPython** - интерпретатор python (среда выполнения); написан на C.

`python3 --version` - Установленная версия.

`.py` - расширение python-файлов.

```
// запуск кода в файле

python3 file.py
```

## Работа с REPL

> Python идёт со встроенным REPL, работает как shell интерпретатор python.

`python3` - запуск REPL.

`help()` - справка в интерактивном режиме.  
- `quit/CTRL+D` - выход  
- `topics` - полезные топики  
- `f/b` - навигация  
- `:q`

**Docstring** - документирование кода, доступно потом в help().

```
// пример документирования кода

def add(a, b):
  """Add a to b function"""
  return a + b

>>> help(add)  // выведет "Add a to b function"
```

`len("string")` - длина строки.

`>>>` - приглашение REPL.

`pepr()` - выведет аргументы в каноническом представлении.

`import` - подключение встроенных модулей в REPL.

```
import operator
from math import sqrt

operator.pow(2, 2)
sqrt()
```

`_` - оператор; хранит результат выполнения предыдущей команды.

___

## Пакеты и индекс

> **Пакет** - единица обмена кодом между разработчиками.

> Python-пакет содержит исходный код и метаданные (описание, версии, совместимость с python, лицензия, зависимости).

> **Индекс** - репозиторий пакетов (обычно веб-сайт).

**PyPI** - python package index; самый популярный индекс;  
`pypi.org`  
`test.pypi.org` - тестовый индекс для обучения.

___

## Distutils, Setuptools, pip

**distutils** - для создания дистрибутивов.

> Каждый пакет содержит `setup.py`, к котором импортирована функция `setup`  из модуля distutils.

**Setyptools** - более удобная надстройка над distutils.

**Poetry** - более простой инструмент для создания пакетов, загрузки в индекс.

```
// установить пакет

python3 setup.py install
```

**pip** - python package installer; для скачивания и установки пакетов.

```
// установить в user окружение

python3 -m pip install --user cowsay
```

## Pip install

**Установка pip**
```
sudo apt update
sudo apt install python3-pip
```

> Pip использует pypi индекс по дефолту.

`python3 -m` - запуск модуля.

`python3 -m pip help` - справка.

**Команды pip:**
- `--version`
- `help`
- `install`:
  - `--user` - установка в user окружение
  - `--upgrade <package>` - обновление версии пакета (можно и сам pip)
  - `--index-url <url>` - адрес альтернативного индекса (pypi по дефолту)
  - `--extra-index-url <url>` - адрес дополнительного индекса, если в `--index-url` не нашёл

```
// обновить pip, если есть версия свежее

python3 -m pip install --user --upgrade pip
```

```
// установка пакета из другого индекса

python3 -m pip install --user --index-url https://...
```

```
// установка пакета из GitHub

python3 -m pip install --user git+https://some-repo.git   // url который используется при git clone
```

___

## Виртуальное окружение


