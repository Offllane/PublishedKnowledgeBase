### Описание
Статический метод `Array.isArray()` проверяет, является ли переданный аргумент массивом. Возвращает `true`, если является, и `false` — если нет.
```js
const arr = [1, 2, 3];  
const emptyArr = [];  
const bool = true;  
const num = 5;  
  
console.log(Array.isArray(arr)); // true  
console.log(Array.isArray(emptyArr)); // true  
console.log(Array.isArray(bool)); // false  
console.log(Array.isArray(num)); // false
```

### Параметры
`Array.isArray()` принимает один аргумент — переменную или значение, которое вы хотите проверить.
Метод возвращает `true` при любом переданном массиве, неважно, как он был создан и какие данные в нём находятся.

### Возвращаемое значение
Метод возвращает false при переданных массивоподобных элементах ([[Псевдомассивы|псевдомассивах]]). Например, на NodeList, HTMLCollection, arguments.
```js
const nodes = document.querySelectorAll('div');  
console.log(Array.isArray(nodes)); // false
```