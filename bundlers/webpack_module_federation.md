`npx create-mf-app` - утилита создаёт преднастроенное mf приложение.
___

`react-app-rewired`, `craco` - позволяют работать с конфигом webpack без `eject` всего cra проекта.
___

## alias in import

```
// webpack.config.js
const path = require('path');

module.exports = {
  ...
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  }
};
```
___
