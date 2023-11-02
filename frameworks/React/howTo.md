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


