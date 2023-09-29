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

