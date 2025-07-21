**typeHinting** - подсказка типов в IDE.
___

## as const

`as const` - создаёт readonly tuple из массива.

```
// as const сделает tuple - массив ограниченной длины с конкретными свойствами
const args = [8, 5] as const;

// type 'hello'
const x = 'hello' as const;

// type { readonly text: 'text' }
const y = { text: 'text' } as const;

// type readonly[10, 20]
const z = [10, 20] as const;
```
___

## Union-type from interface

```
type T = keyof Date;

type T2 = keyof typeof obj;  // тоже из ключей объекта
```
___

## Generic names

By convention, use uppercase single-letter names starting with the letter T then U, V, W, X, Y, Z.
___

Дженерик типу не обязательно передавать руками дженерик-параметр, TS сам может вывести тип из того как оно используется
внутри.
___

TS проверяет совместимость объекта по структуре, а не по принадлежности к требуемому интерфейсу или классу.
Исключение инстансы классов с private, protected, эти типизируются не по идентичности структуры, а по наследству.
___

Enum и class уникальны тем что это и тип и значение, напр. enum можно использовать для типизации переменной, так и
для присвоения значения содержащегося в enum.
___

Generic type parameter может иметь дефолтное значения типа, тем самым становится необязательным.
___

typeof в TS выводит тип из аннотации, а typeof в JS выводит тип из значения, а так одна фигня.
___

## Тип по ключу

```
type FriendList = APIResponse['user']['friendList'];
type Friend = FriendList['friends'][number];

function get<
  O extends object,
  K extends keyof O
>(
  o: O,
  k: K
): O[K] {
  return o[k]
}

// with overloading
type Get = {
  <
    O extends object,
    K1 extends keyof O
  >(o: O, k1: K1): O[K1]
  <
    O extends object,
    K1 extends keyof O,
    K2 extends keyof O[K1]
  >(o: O, k1: K1, k2: K2): O[K1][K2]
  <
    O extends object,
    K1 extends keyof O,
    K2 extends keyof O[K1],
    K3 extends keyof O[K1][K2]
  >(o: O, k1: K1, k2: K2, k3: K3): O[K1][K2][K3]
};

const get: Get = (object: any, ...keys: string[]) => {
  let result = object;
  keys.forEach(k => result = result[k]);

  return result;
};
```
___

## keyof

`keyof` - returns union type of all keys in object type.

```
type ResponseKeys = keyof APIResponse // 'user';
type UserKeys = keyof APIResponse['user'] // 'userId' | 'friendList';
type FriendListKeys = keyof APIResponse['user']['friendList'] // 'count' | 'friends';
```
___

## Mapped type

```
type MyMappedType = {
  [Key in UnionType]: ValueType
};

type Account = {
  id: number
  isEmployee: boolean
  notes: string[]
};

// Make all fields optional
type OptionalAccount = {
  [K in keyof Account]?: Account[K]
};

// Make all fields nullable
type NullableAccount = {
  [K in keyof Account]: Account[K] | null
};

// Make all fields read-only
type ReadonlyAccount = {
  readonly [K in keyof Account]: Account[K]
};

// Make all fields writable again (equivalent to Account)
type Account2 = {
  -readonly [K in keyof ReadonlyAccount]: Account[K]
};

// Make all fields required again (equivalent to Account)
type Account3 = {
  [K in keyof OptionalAccount]-?: Account[K]
};
```

`-` - (minus) is a special operator for mapped type.

In fact `Record`, `Partial`, `Required`, `Readonly`, `Pick` - is mapped type under hood.
___

## is

`is` - user-defined type guard.

```
function isString(a: unknown): a is string {
  return typeof a === 'string';
}
```
___

## Conditional types

```
type TIsString<T> = T extends string ? true : false;

type TWithout<T, U> = T extends U ? never : T;

// like mapping through all union type
type A = TWithout<
  boolean | number | string,
  boolean
>; // number | string

type TElement<T> = T extends (infer U)[] ? U : T;
```

```
type TSecondArg<F> = F extends (a: any, b: infer B) => any ? B : never;

type F = typeof Array['prototype']['slice'];

type A = TSecondArg<F>; // number | undefined
```
Built-in conditional types:
 - `Exclude<UnionType, ExcludedMembers>` - constructs a type by excluding members from union.
 - `Extract<Type, Union>` - constructs a type by extracting from Type all assignable to Union members.
 - `NonNullable<T>` - excludes `null`, `undefined` from type.
 - `ReturnType<F>` - function's return type.
 - `Parameters<F>` - tuple of function's parameters.
___
