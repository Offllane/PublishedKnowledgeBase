Изменяет исходный массив
Возвращает изменный массив

## Описание
Метод `fill()` заполняет часть или все элементы массива одним и тем же значением.
Метод `fill()` — удобный способ заполнить массив одним и тем же значением. Обычно это необходимо при инициализации массива.

Заменить все значения
```js
const colors = ['red', 'green', 'white'];
colors.fill('purple');

console.log(colors);
// ['purple', 'purple', 'purple']
```

```js
const skills = ['HTML', 'CSS', 'JS', 'Python']; 
skills.fill(null, 2);

console.log(skills) // ['HTML', 'CSS', null, null]
```

```js
const createNullArr = (length) =>
{
  const array = Array(length)
  return array.fill(null)
}

console.log(createNullArr(5))
// [null, null, null, null, null]
```

> [!warning] Не заполнять с помощью fill объектами
> Если значение, используемое для заполнения, является объектом, метод `fill()` будет использовать для заполнения ссылку на этот объект.

Лучше делать вот так
```js
const persons = Array.from( new Array(3), () => ({ name: 'имя', position: null }))
persons[0].name = 'София'

console.log(persons)
// [
//   { name: 'София', position: null },
//   { name: 'имя', position: null },
//   { name: 'имя', position: null }
// ]
```

