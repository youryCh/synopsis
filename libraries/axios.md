## axios.get

`axios.get(url, { params })`  

`params` - объект с query-параметрами; axios сам пропишет через `?key=value&key2=value2`

`baseURL` - также можно передать в объекте params; для запроса на другой url, не тот который указан при axios.create().

___

## Interceptors

> Позволяет перехватывать запрос/ответ и делать с ним что-то до отправки.  

```
// внедрить токен аутентификации
service.interceptors.request.use(
  (config) => {
    config.headers.common.Authorization = `Bearer ${ token }`;

    return config;
  }
);
```

___

