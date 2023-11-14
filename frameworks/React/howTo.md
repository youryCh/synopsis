## Добавить читаемое имя чанка в бандле

```
lazy(() => import(/* webpackChunkName: About */ './About'))

// в результате получится так
<link href="./About.bundle.js" />
```
___

## Использовать sass

`npm i node-sass` - установить; по дефолту cra использует css, less.
___

## Подключить cra-проект к github

1. создать проект через cra
2. создать репу (без readme)
3. `git remote add origin <path_to_repo>`
4. `git push -u origin main` (если по дефолту master - сначала `git branch -m main`)
___
