### Создать новый репозиторий
#### Вариант 1:
- создать локальный репозиторий (git init)
- создать новый удалённый репозиторий (без readme), копировать ssh
- в локальном репозитории:
  ```
  git remote add origin <ssh>
  git branch -M main
  git push -u origin main
  ```
  `git branch -M main` - переименование ветки master в main (BLM reasons); с какой-то новой версии git это не нужно

#### Вариант 2:
- создать рабочую директорию (имя будет именем репозитория)
- из этой директории `git init`
- добавить README.md
- `git add .`
- `git commit -m 'Initial'`
- варианты пуша:
    - кнопка Publish branch в VSC (перенаправит в Github, предложит опубликовать public/private repo)
    - `git remote add origin <remote_repo_url>` + `git push -u origin main/master`
___

### Заменить локальную ссылку на remote repo
`git remote -v` - посмотреть текущий url.

**2 варианта замены:**
1. в linux:
   `sed -i 's/что/на_что/g' .git/config` - удобно для частичной замены например хоста, а не всего урла sed найдёт значение по шаблону и заменит.  
   Без `-i` изменит только вывод в консоль, удобно для теста изменений.

   ```
   // example
   sed -i 's/github.com/github.enterprise.tech/g' .git/config
   ```
2. `git remote set-url origin git@github.com/...repo.git`

___

### Cherry-pick

Позволяет перенести любой коммит на текущую ветка.

Как работает:
1. Находим нужный коммит на Github или через список локально (`git log --oneline`)
2. Выполняем `git cherry-pick <hash>` на ветке, в которую надо перенести коммит.

___

## Доступ к git info из кода

```
process.env.GIT_VERSION
            GIT_COMMITHASH
            GIT_BRANCH
```

___

## Push error

Без этих настроек не будет доступа пушить в удалённый репозиторий.

`git config --global user.name 'name'`

`git config --global user.email 'email'`

___

## Добавить SSH-key

В bash:
- `cd ~`
- `ssh-keygen` (сгенерит пару приватный/публичный)
- `cat ~/.ssh/id_rsa.pub | clip` (либо руками скопировать из id_rsa.pub)

В GitHub/GitLab:
- открыть Settings (Edit profile в GitLub)
- в SSH keys создать новый ключ и вставить содержимое паблик-ключа

> Текущий ключ можно посмотреть в `C:/users/yuri/.ssh/id_rsa.pub`

___

## Быстро клонировать репозиторий

- копировать ссылку (SSH/HTTP) нужного репозитория
- создать директорию с названием проекта
- ПКМ -> Git Bush Here
- `git clone <вставить_ссылку>`

___

## Как править merge конфликты

- смотрим конфликты в GitLab
- в VSC пулим origin-ветку, от которой чекаутили
- в Source Control тыкаем файлы с конфликтами (! справа)
- лучше принять income changes и добавить свои изменения (не всегда), сохраняем файл
- когда всё зарезолвлено, стейджим всё через жирный `+`
- commit message 'Resolve merge conflicts'
- пушим в remote repo
- мержим через GitLab

___

## Как подтянуть изменения мастер-ветки

> Если мастер-ветка менялась после чекаута фича-ветки, то лучше подтянуть изменения и порешать возможные конфликты локально, перед MR/PR.

`git pull origin develop`

___

## Поиск в GitHub

`/` - откроет строку поиска; можно искать по репозиторию или по всем репам.

___

## Сделать пустой коммит

`git commit -m 'message' --allow-empty` - пустой коммит без изменений; например стриггерить деплой.

___

## Rebase

- `git switch main` - to get back on the main branch
- `git pull` - to get most up-to-date version of the main branch
- `git switch feature` - to get back on the feature branch
- `git rebase main`- to perform rebase; with `-i` - interactive rebase? where you can pick commits, rename, squash and more.

___

## Pre-commit error

- удалить директорию `.git/hooks`
- `npm rebuild`

___


