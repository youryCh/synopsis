**GitHub Actions** - бесплатная для публичных репозиториев система CI.

`.github/workflows` - директория внутри репозитория, содержит файлы с описанием шагов в yml формате.

`Workflow` - процесс выполнения описанных шагов CI; определяется в файле конфига в `.github/workflows`;  
несколько workflow могут выполняться параллельно.

`Events` - workflow может запускаться одним или несколькими событиями (push, release, PR, cron).

`Jobs` - workflow состоит из одного/нескольких заданий; job содержит набор команд.

`Runners` - временный сервер на GitHub с выбранной ОС; каждая ждоба выполняется в ранере.

`Steps` - джоба состоит из последовательности шагов.

`Actions` - выполняет задачу; переиспользуемый блок кода.



`on` - определяет события, которые запускают workflow.

`runs-on` - ОС для работы workflow.

`checkout` - клонирует репозиторий.

```
-uses: actions/checkout@v3
```

`run` - произвольная баш-команда.

```
-run: ls -la
```

## Makefile

`Makefile` - документация и исполняемый код; хранится в корне проекта.

`make` - утилита; запускает команды в Makefile через шорткаты.

**Синтаксис Makefile**
```
target1:       // target name (kebab-case/snake_case)
  command1     // выполняются последовательно; если ошибка в command1, то command2 не выполнится
  command2
```

```
install:
  composer install
  npm i

// можно запустить через make install
```

```
// зависимости задач
start: install    // make install сначала выполнит install
  npm start

install:
  npm i

// по сути тоже что make install start
```

> `make` изначально сделана для сборки кода из файла, поэтому она ищет файл с переданным именем  
> и пытается его собрать, чтобы этого не было надо прописать `.PHONY`.

```
test:
  php artisan test

.PHONY: test   // иначе make test найдёт папку /test и упадёт с ошибкой
```

```
// пример обработки ошибок
env-prepare:
  cp -n .env.example .env || true   // если файл уже существует, то без || true, код упадёт
 ```

```
// прокидывание переменной
# Makefile
USER_VAR=unknown  // значение по у молчанию

say:
  echo "Hello, $(USER_VAR)!"

# Bash
make say USER_VAR=Anna
```

`$(CURDIR)` - в Makefile это `PWD`.

___

### Jobs

> По дефолту запускаются параллельно, но можно упорядочить.

```
jobs:
  build-frontend:          //  запустятся параллельно
  build-backend:
  test:                    // запустится после сборки бекенда
    needs: build-backend
```

```
// nodeJS example
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      -uses: actions/checkout@v3
      -uses: actions/setup-node@v3
      -run: npm i
      -run: npm run lint
      -name: run tests        // опционально можно указать шага
        run: npm tests
```

> Каждый щаг запускается в одной и той же директории, туда же клонировать репозиторий.

```
// пример джобы для нескольких ОС
jobs:
  build:
    runs-on: ${{matrix.OS}}
    strategy:
      matrix:
        OS: [ubuntu-latest, macos-latest]
  steps:
    ...
```

> Есть много дефолтных переменных окружения - смотри доку `GITHUB_...`.

```
// переменные окружения
jobs:
  run:
    env:
      PAILS_ENV: staging
      DEBUG: 1
```

___

### Actions

> Экшены работают как один из шагов.  
> Вместо `run` используется `-uses`.  
> Берутся из каталога на GitHib.

`actions/...` - встроенные экшены от GitHub.

**Параметры** задаются через `with`.

```
-uses: actions/setup-node@v3
with:
  node-version: '18.x'
  cache: 'npm'  // ускоряет повторные сборки
```

___

### Добавить секрет в репозиторий

`repo` -> `settings` -> `Secrets and variables` -> `add`

```
// использование в workflow
...
env:
  TOKEN: ${{secrets.TOKEN}}
```

___


