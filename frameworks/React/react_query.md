# React Query

`queryClient()` - используется для работы с кешем; может использоваться в корне приложения (App.tsx) для инициализации настроек кеша.

```
// App.tsx
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      cacheTime: 1000 * 60 * 15,
      retry: false,
      refetchOnWindowFocus: false,
    },
  },
});

const RootApp () => {
  return (
    <QueryClientProvider client={ queryClient }>
      <App />
    </QueryClientProvider>
  );
};
```
___

`useQuery(key, query, options)` - хук для подписки на запросы в компоненте или кастомном хуке; принимает: 
- `key` - уникальный; используется для кеширования, повторных запросов
- `query` - метод запроса; должен возвращать promise
- `options`:  
  - `enabled` - разрешает запрос если true; для зависимых последовательных запросов; можно описать условие выполнения  
    запроса
  - `debounce` - делает запрос с уменьшающейся частотой
  - `retry` - перезапрос при ошибке
  - `refetchOnWindowFocus` - перезапрос в фоновом режиме
  - `refetchOnMount`
  - `cacheTime` - время по истечению которого неактивный запрос будет удалён из кеша; 5 мин по дефолту
  - `staleTime` - время до перехода запроса из свежего в протухший; 0 мс по дефолту; `Infinity` - отключает фоновое  
    обновление
  - `initialData` - данные отображаемые пока идёт запрос
  - `keepPreviousData`
  - etc.

**Query key** - RQ всегда делает перезапрос при изменении ключа, поэтому все переменные, которые входят в запрос,  
хорошо бы передавать в составной ключ (как массив зависимостей в useEffect).

```
const info = useQuery('todo', fetchTodo);

// составной ключ
const { data } = useQuery(['todo', id], fetchData);
```

**useQuery возвращает** объект со свойствами:
- `isLoading` - запрос на сервер (не из кеша)
- `isError`
- `isSuccess` - успех, данные доступны
- `isIdle`
- `isFetching` - запрос выполняется (в т.ч. для кешированных данных)
- `error` - объект ошибки, если isError
- `data` - полученные данные, если isSuccess
- `status` - текущий статус: 'loading', 'error', etc.
- `refetch` - повторный запрос (используется при запросе по клику и пр.)
- etc.

```
const { isLoading, isError, date, error } = useQuery('todo', fetchTodo);

if (isLoading) return <p>Loading...</p>;

if (isError) return <p>Error: {error.message}</p>;

return (
  <ul>
    {data.map((todo) => <li>{todo.title}</li>)}
  </ul>
);
```
___

**Запрос по клику:** делаем `enabled: false` и в хендлере клика юзаем refetch; забавно что получается refetch игнорит  
флаг enabled - тут надо быть осторожнее.
___

`useQueries()` - для нескольких запросов.

`useIsFetching()` - возвращает количество запросов в фоновом режиме (например для глобального индикатора загрузки).

`useInfinityQuery()` - для бесконечных запросов (например подгрузка при скролле).
___

## isLoading vs isFetching

Используется для лоадеров, но есть нюансы:

`isFetching` будет true даже если данные грузятся из кеша.

`isLoading` будет true только при получении данных с сервера.
___

## Mutation

`useMutation()` - хук для создания/удаления/изменения/обновления данных.

Возвращает объект с:
- `isIdle`
- `isLoading`
- `isError`
- `isSuccess`
- `mutate()` - для запуска мутации; принимает объект/примитив.
- `mutateAsync()` - то же, но возвращает promise.
- `reset` - сброс при ошибке
- `data`
- `error`
- `status`

```
const mutation = useMutation((newTodo) => axios.post('./todo', newTodo));

return (
  <>
  {mutation.isLoading && <p>Loading...</p>}
  <button
    onClick={() => mutation.mutate({title: 'Hello'})}
  >
    AddTodo
  </button>
  </>
);
```
___
