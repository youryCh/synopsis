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

## Настроить доступ в приватный регистр

1. Создать `.npmrc` в корне проекта.
2. ```
    --init-author-name=John Doe
    --init-author-email=john.doe@corp.pro
    email=your.name@corp.pro
    always-auth=true
    registry=https://private-registry-url/
    //same-registry-url-without-https/:_auth=your_credentials_in_base64
   ```
   Все эти настройки можно установить через:
    -  `npm set https://private-registry-url`
    -  `npm set _auth=base64_credentials`
3. `echo -n "login:password" | openssl base64` - кодировать учетку

`npm login -scope=@your_compony --registry=https://path/` - ещё вариант авторизации, запросит логин/пароль.
___
