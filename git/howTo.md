### Создать новый репозиторий
- создать локальный репозиторий (git init)
- создать новый удалённый репозиторий (без readme), копировать ssh
- в локальном репозитории:
  ```
  git remote add origin <ssh>
  git branch -M main
  git push -u origin main
  ```
  `git branch -M main` - переименование ветки master в main (BLM reasons); с какой-то новой версии git это не нужно

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

