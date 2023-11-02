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


