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

