> [**no-plusplus**](https://eslint.org/docs/latest/rules/no-plusplus) - правило линтера, запрещает унарный плюс/минус, т.к. auto semicolon insertion может работать некорректно; предлагает использовать +=/-=

```for (let i = 0; i < 10; i += 1) {}```
___