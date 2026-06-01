Не изменяет исходные массивы.
Возвращает новый массив.
### Описание
Объединяет несколько массивов.
```ts
a.concat(b);
[].concat(a, b);
```

В отличие от [[Array.splice()]] создает новый массив, не изменяя текущий.
В качестве аргументов принимает не только массивы.
```ts
const numbers = [1, 2, 3]
const result = numbers.concat(
  4, 'five', null, {name: 'six'}, [[7]]
);

console.log(result);
// [1, 2, 3, 4, 'five', null, {name: 'six'}, [7]]
```

Symbol.isConcatSpreadable = false -- элемент добавиться как отдельный
Symbol.isConcatSpreadable = true -- элемент добавиться как ожидается 
```ts
const numbers = [1, 2, 3]

const otherNumbers = [4, 5, 6]
otherNumbers[Symbol.isConcatSpreadable ] = false

console.log(numbers.concat(otherNumbers))
// [1, 2, 3, [4, 5, 6, [Symbol(Symbol.isConcatSpreadable)]: false ]]

const numbers = [1, 2, 3]
const arrayLike = {
  [Symbol.isConcatSpreadable ]: true,
  '0': 4,
  '1': 5,
  '2': 6,
  length: 3
}

console.log(numbers.concat(arrayLike))
// [1, 2, 3, 4, 5, 6]
```
