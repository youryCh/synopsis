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

## How to share state between mf modules

1. Creatig store (effector) in root app
```
// root app
// src/stores/items.model.ts
import {createStore, createEvent} from 'effector';

export const setItems = createEvent<IItem[]>();

export const $items = createStore<IItem[]>([])
  .on(setItems, (_, payload) => payload);

export const ItemsStore = {
  $items,
  setItems
};
```
2. Bootstraping store
```
// root app
// src/exposes/ItemsStore.ts
import {ItemsStore} from '@/stores/items.model';

export const {$items, setItems} = ItemsStore;
```
3. Exposing store from the root app
```
// webpack.config.js
new ModuleFederationPlugin({
  name: 'root_app',
  filename: 'remoteEntry.js',
  exposes: {
    './ItemsStore': './src/exposes/ItemssStore.ts'
  },
  shared: {
    ...
    'effector'
  }
  ...
})
```
4. In the root app we can use simple import
```
import type {FC} from 'react';
import {useUnit} from 'effector-react';

import {$items} from '@/stores/items.model';

export const Component: FC = () => {
  const items = useUnit($items);

  return <SubComponent items={items} />;
};
```
5. Adding remotes in the mf app
```
// mf app
// webpack.confog.js
new ModuleFederationPlugin({
  name: 'mf_app',
  filename: 'remoteEntry.js',
  remotes: {
    root: 'root_app@http://localhost:3000/remoteEntry.js'
  },
  exposes: {...},
  ...
})
```
6. Using store in the mf app
```
// some component
import {setItems, $items} from 'root/ItemsStore';

export const AnotherComponent: FC = () => {
  const items = useUnit($items);

  useEffect(() => {
    setItems([]);
  }, []);
};
```

### To avoid async/await module import use this:
```
// webpack.config.js
 target: ['web', 'es6'],
  entry: './src/index.ts',
  mode: process.env.NODE_ENV || 'development',
  output: {
    filename: 'bundle-[contenthash].js',
    globalObject: 'this',
    path: path.resolve(__dirname, 'dist'),
    publicPath: 'auto',
    environment: {
      asyncFunction: true  <------
    }
  },
```
___
