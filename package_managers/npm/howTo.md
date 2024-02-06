## Обновить npm до последней версии

`npm i -g npm@latest`
___

## Поставить nvm

> На `geeksforgeeks.org` есть дистрибутив `nvm-setup.zip`, распаковать, запустить installer.

`nvm list` - чекнет доступные версии node.

`nvm current` - текущая версия.

`nvm help`

`nvm install 14` - установить последнее минорное обновление 14 ноды.

`nvm install lts` - установить последнюю стабильную версию.

`nvm use 14` - переключиться на ^14 версию, либо точно указывать версию.

Bash запускать as Admin.
___

## Изменить адрес реестра пакетов

`npm config list` - посмотреть текущий конфиг npm.

`npm config set registry <regictry_url>` - установить дефолтный реестр.

`npm config set @<SCOPE>:registry <registry_url>` - for scoped packages.
___
