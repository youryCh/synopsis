# Electron + microfrontends

Недавно на проекте столкнулся с необычной задачей - сделать из готового React веб-приложения десктопную версию
на Electron. Что же тут необычного? А то, что наше веб-приложение построено на микрофронтенд архитектуре
и располагается в трёх отдельных репозиториях. А общение между микрофронтендами происходит в runtime через HTTP.
И тут начинаются сложности, так как для создания дистрибутива, Electron'у нужен доступ к исходникам всего
приложения. Хотя Electron легко [подружить с Webpack](https://www.electronforge.io/templates/webpack-template),
как это сделать с плагином Module Federation на первый взгляд не понятно.

Поиск готового решения в интернете ничего не дал, кроме повисших в воздухе вопросов на Stack Overflow. Пришлось
придумать своё решение, которое я и опишу здесь.

Стек проекта типовой (React, Webpack Module Federation, Electron, Electron-forge), поэтому не буду подробно
расписывать конфиги, лишь опишу ключевые моменты.

## Developer build
Начнём с локального запуска десктопного приложения в develop режиме. Здесь всё просто - нужно только изменить
скрипт запуска. Хитрость в том, чтобы параллельно запустить корневое приложение в дев режиме и electron-forge.

```
// package.json
// Скрипт ожидает запуск корневого приложения в режиме разработки и запускает Electron.

"scripts": {
  ...
  "desktop:start": "concurrently \"yarn start\" \"wait-on tcp:3003 && electron-forge start\""
}
```

А что же с ремоутами? Их придётся стартовать вручную в отдельных терминалах. Это простое решение подходит только
для режима разработки, с продуктовой сборкой немного сложнее.

## Production build

Здесь добавляется немного ручной работы, так как на момент упаковки `electron-forge package` в корневом репозитории
должны лежать все необходимые бандлы ремоутов. Как же получить бандлы? Пока я не нашёл другого решения, кроме как
тащить руками. Для этого идём в ремоут приложение, делаем прод сборку и копируем `/dist` в корень хост приложения.

Далее нужно включить бандл ремоута в дистрибутив десктопного приложения. Для этого указываем forge где лежат эти
ресурсы:

```
// forge.config.js

module.exports = {
  packagerConfig: {
    extraResource: [
      './your-mf-module'
    ]
  }
}
```

Далее нужно внедрить `remoteEntry.js` скрипт в `index.html` рута и инициализировать микрофронтовый модуль.
Для этого в `preload.js`, где есть доступ к node api, опишем функцию внедрения скрипта и положим её в
глобальный `window`:

```
// preload.js

contextBridge.exposeInMainWorld('electron', {
  initMf: async () => {
    const localAppPath = await ipcRenderer.invoke('get-local-app-data');
    const pathToScript = path.join(localAppPath, 'App-name', 'app-1.0.0', 'resources', 'your-mf-module', 'dist', 'remoteEntry.js');
    const script = document.createElement('script');

    script.src = pathToScript;
    document.head.appendChild(script);
  }
});
```

Тут есть одна сложность - нужно определить путь до директории локальной установки приложений (`LOCALAPPDATA`).
Используем `ipcRenderer.invoke()` чтобы получить данные из main process. Также опишем соответствующий хендлер
в `main.js`:

```
// main.js

const createWindow = () => {
  const mainWindow = new BrowserWindow({...});

  ipcMain.handle('get-local-app-data', () => process.env.LOCALAPPDATA);
};
```

Вызов функции `initMf` происходит в `renderer.js`, где доступен объект `window`:

```
// renderer.js

window.electron.initMf();
```

Таким образом мы добавляем скрипты ремоут модулей при старте приложения. Далее их нужно инициализировать как
webpack модули. В документации Webpack есть [пример](https://webpack.js.org/concepts/module-federation/) такой
функции, используем её:

```
function loadComponent(scope, module) {
  return async () => {
    // Initializes the shared scope. Fills it with known provided modules from this build and all remotes
    await __webpack_init_sharing__('default');

    const container = window[scope]; // or get the container somewhere else

    // Initialize the container, it may provide shared modules
    await container.init(__webpack_share_scopes__.default);

    const factory = await window[scope].get(module);
    const Module = factory();

    return Module;
  };
}
```

При вызове эта функция находит модуль в `window`, инициализирует и возвращает его. Полученный модуль импортируем
в корневой `App.tsx` через `React.lazy()`.

Вот собственно и всё решение. Теперь можно без проблем упаковать десктопное приложение и собрать дистрибутив.

## Заключение

Возможно, это решение не самое изящное, но кажется единственная альтернатива ему - глобальная
перестройка архитектуры всего приложения в монорепозиторий. А в моём решении нужны только доработки в хост
приложении и никаких изменений в ремоутах. А в ремоутах как раз и происходит вся разработка, и для обновления
десктопной версии приложения нужен только новый бандл изменённого ремоута - и свежий дистрибутив готов.

## Теги: Frontend, frontend-разработка, Electron, Webpack Module Federation
