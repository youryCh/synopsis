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