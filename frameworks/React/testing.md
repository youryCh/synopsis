## Тестирование

Тесты бывают:
- Ручные
- Автотесты:
  - e2e
  - unit-test
  - сервисные
  - интеграционные
  - компонентные
  - скриншот-тесты
  - снапшот-тесты

**Unit-test** - тестирование работы каждой не тривиальной фичи.
___

## Jest

Библиотека для юнит-тестирования реакт компонентов.  
Используется в cra по дефолту.

`npm test` - запуск тестов.

`npm test example` - запуск тестов по маске; запустит тесты для example.js

**Префиксы командной строки:**
- `a` - run all tests
- `f` - only failed tests
- `o` - тестировать файлы, в которых произошло изменение
- `p` - filtering by file name
- `t` - filtering by test name

- `enter` - запустить/прервать тесты.
- `q` - выход из watch mode.

**Способы конфигурации:**
- в package.json поле jest
- в jest.config.js
- через флаги командной строки

**test suite** - набор тестов в одном файле.
___

### Синтаксис

`describe()` - функция группировки тестов.

`it()/test()` - принимает строку с описанием и callback с тестом.

`expect()` - возвращает объект с кучей методов (matchers) для оценки и сравнения результата выполнения теста.

```
describe('formatTimeString test', () => {
  it('returns None if no opening hours passed', () => {  // it() == test()
    const expected = 'None';
    const received = formatTimeString([]);

    expect(received).toEqual(expected);
  });
});
```
___

### Matchers

- `toBe()` - сравнение `==`
- `toEqual()` - сравнение `===`
- `toDeNull()`
- `toBeDefined()`
- `toBeTruthy()`
- `toBeFalsy()`
- `toHaveBeenCalled()`
- `toHaveBeenCalledTimes()` - показывает количество вызовов (проверить дубли)
- `toThrow()` - было ли выброшено исключение при работе функции
- `toContain()` - есть ли в массиве элемент
___

`test.skip('', () => {})` - пропустить конкретный тест.

`jest.fn()` - заглушка для функций.

`jest.mock()`

```
render(
  <Provider store={{
    getState: jest.fn(),
    dispatch: jest.fn()
  }}>
);
```

```
// пример мока для redux
jest.mock('react-redux', () => ({
  useDispatch: jest.fn()
}));
```

## React-testing-library

`render()` - метод виртуальной отрисовки; принимает компонент; возвращает пререндеренный компонент.

`toMatchSnapshot()` - матчит новый snapshot полученный из render(), с имеющимся.

```
describe('App tests', () => {
  it('matches snapshot online', () => {
    const component = render(<App />);

    expect(component).toMatchSnapshot();  // снепшоты по дефолту лежат в директории с тестами
  });
});
```

`screen` - делает скриншот отрендеренного компонента для сравнения.

`getByText()` - метод для сравнения скриншотов; принимает string | regExp.

```
it('message should contain text Hello', () => {
  render(<Message message={testMessage} />);

  const wrapper = screen.getByText(/hello/i);

  expect(wrapper).toBeInTheDocument();
});
```
___
___
