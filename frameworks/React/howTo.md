## Добавить читаемое имя чанка в бандле

```
lazy(() => import(/* webpackChunkName: About */ './About'))

// в результате получится так
<link href="./About.bundle.js" />
```

___


