# How to:

## Electron + Webpack Module Federation

Суть в том чтобы конкурентно запустить два процесса: webpack сборку и пакетирование электрона. Сделать это нужно в скрипте.
Ну и куча спицефических настроек webpack и electron-forge.

```
// примерно така:
"scripts": {
    "start": "webpack serve --node-env=development",
    "electron:start": "concurrently \"cross-env yarn start\" \"cross-env NODE_ENV=development wait-on http://localhost:3003 && electron-forge start\"",
    "electron:package": "electron-forge package"
  }
```
____
