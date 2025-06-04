## Типизировать useState

`useState<Record<string, ISomething>>({});`
___

## Fix 'Index signature missing in type' error

Как я понял требует типизировать ключ в объекте, например для типизации useParams реакт роутера.

```
interface IParams {
  [someKey: string]: string;
}

const {someKey} = useParams<IParams>();
```
___

## Fix nested fc typing error

The error appears when using HOC in React.

1. `"@emotion/react": "11.11.1"` - this affects
2. `"@types/react": "^18.3.18"`
___
