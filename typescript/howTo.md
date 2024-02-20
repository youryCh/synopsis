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
