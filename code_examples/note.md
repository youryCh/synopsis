## Compare objects

```
const getObjectsDifference = (firstObject, secondObject) => Object
  .entries(firstObject)
  .reduce((acc, [ key, value ]) => {
    const result = isEqual(value, secondObject[key]) ? {} : {[key]: secondObject[key]};

    return {...acc, ...result};
  });
```

___

## CSS in Styled Components

```
const iconStyles = css`
  width: 16px;
  height: 16px;
  cursor: pointer;
`;

const StyledIcons = styled(FilledFilter)`
  ${iconStyles};
  color: ${({ theme }) => theme.palette.colors.green};
`;
```

___

## Simple custom hook example

```
export const useRate = () => {
  const [rate, setRate] = useState(1);

  const handleRating = (rate: number) => {
    setRate(rate);
  };

  return { rate, handleRating };
};
```

___

## Custom hook example

```
export const useProducts = () => {
  const [products, setProducts] = useState<any[]>([]);

  const fetchProducts = async () => {
    const response = await axios.get(
      'https://fakestoreapi.com/products'
    );

    if (response && response.data) {
      setProducts(response.data);
    }
  }

  useEffect(() => {
    fetchProducts();
  }, []);

  return { products };
};
```

___

## Env usage. Zustand

```
import { create, StateCreator, UseBoundStore, StoreApi } from 'zustand';
import { devtools } from 'zustand/middleware';

export const createStore = <T>(
  fn: StateCreator<T>,
  name: string,
): UseBoundStore<StoreApi<T>> => {
  if (process.env.NODE_ENV === 'development') {
    return create(
      devtools(fn, { name }),
    );
  }

  return create(fn);
};
```

___

## Get yesterday

```
export const getYesterday = (): Date => {
  const date = new Date();

  date.setDate(date.gatDate() - 1);

  return date;
};
```

___

## isOnline react hook

export const useDetectOnLine = () => {
  const [online, setOnline] = useState(true);

  useEffect(() => {
    setOnline(navigator.online);

    const onlineListener = () => setOnline(true);
    const offlineListener = () => setOnline(false);

    window.addEventListener('online', onlineListener);
    window.addEventListener('offline', onlineListener);

    return () => {
      window.removeEventListener('online', onlineListener);
      window.removeEventListener('offline', onlineListener);
    };
  }, []);

  return online;
};

___

## makeStyles usage

```
// styles.ts
import { makeStyles } from '@material-ui/core'

export const useStyles = makeStyles({
  title: {
    minWidth: 144,
  },
})

// component
import { useStyles } from './styles'

const Component = () => {
  const classes = useStyles()

  return <div className={classes.title}></div>
}
```

___

## Props in Styled components

```
interface IExtendedProps extends IProps {
  flexGrow: number;
}

const StyledForm = styled(Form)<IExtendedProps>`
  flex: ${({ flexGrow }) => flexGrow} 1 0;
`;
```

___

## Reduce with Map

```
export const getRowsMap: GetRowsMap = (graphics) => graphics.reduce(
  (acc, g) => acc.set(g.shift, [...(acc.get(g.shift) || []), g]),
  new Map<SHIFT, ChoiceGraphicResponse[]>(),
);
```

___

## String normalizer

```
const normalizeString = (string) => {
  const symbolsMap = new Map([
    ['&', '&amp;'],
    ['<', '&lt;'],
    ['>', '&gt;'],
    ['"', '&quot;'],
    ["'", '&#39;']
  ]);
  const regexp = /[&<>"']/g;
  const hasSymbols = RegExp(regexp.source).test(string);

  return string && hasSymbols
    ? string.replace(regexp, (char) => symbolsMap.get(char))
    : string || '';
};
```

___


