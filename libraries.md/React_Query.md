# React Query

> **Query Client** - используется для работы с кешем.  
> Может использоваться в корне приложения (App.tsx) для инициализации настроек кеша.

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

