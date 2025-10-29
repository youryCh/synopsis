## To mock import

This allows to avoid import module (like `import Some from '@somewhere';`). Also to mocking MF module.

1. Add the imported module to the mapper in the config
```
// jest.config.ts
  ...
  moduleNameMapper: {
    'some-module': '<rootDir>/__mocks__/someModuleMock.ts'
  }
```

2. Create mock file
```
// __mocks__/someModuleMock.ts
import {jest} from '@jest/globals';

export default {
  someHandler: jest.fn(() => {}),
  someProp: 'test'
}

// or just function
import {jest} from '@jest/globals';

export const setSomething = jest.fn(() => {});
```
___
