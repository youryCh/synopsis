**Исчезающий фреймворк**:
- убирает лишние абстракции и вычисления из рантайма браузера (нет виртуального DOM), всё делает в момент компиляции;  
увеличивает производительность
- в итоге получаем ванильный js код; упрощает миграцию кода
___

Отличия Svelte:
1. все компоненты .svelte это валидные html компоненты (т.е. file.svelte можно переделать в file.html и будет работать)
2. нет virtual DOM
3. реактивность работает на нативном js (т.е. в результате компиляции получается императивный код, который и меняет DOM)
___

Svelte хорошо подойдёт для:
- производительный приложений: браузерные расширения, смарт тв, для слабого железа
- поддержка и масштабирование легаси проектов (JQuery и пр)
___

- `/public` - contains files which will not be used in compilation.
- `main.js` - creates new svelte instance.
- `scripts`  -contains file for typescript setup.
___

Svelte supports typescript.
___

Svelte component consist of:
- `<script>` - for business logic
- `<main>` - for html markup
- `<style>` - for css
___

Интерполяция переменных из скрипта делается через `{}`.
___

Если переменная в скрипте указана с `export`, то svelte считает её как prop.

```
export let name;  // можно присвоить дефолтное значение через =
```
___


