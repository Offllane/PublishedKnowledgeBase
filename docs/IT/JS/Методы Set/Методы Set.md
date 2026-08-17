## `union` - объединяет текущую коллекцию с другой и возвращает новую, состоящую из элементов, которые есть в любой из коллекций.
```js
const project1 = new Set(['PHP', 'Bash', 'Docker', 'JavaScript', 'CSS'])
const project2 = new Set(['Docker', 'JavaScript', 'Bash', 'Postgres', 'MongoDB'])

const result = project1.union(project2)

console.log(result)
// Set(7) {
//   'PHP',
//   'Bash',
//   'Docker',
//   'JavaScript',
//   'CSS',
//   'Postgres',
//   'MongoDB'
// }
```

## `difference()` сравнивает текущую коллекцию с другой и возвращает новую, состоящую из элементов, входящих только в первую коллекцию.
```js
const names1 = new Set(['Аглая', 'Настасья', 'Елизавета', 'Соня'])
const names2 = new Set(['Лев', 'Родион', 'Настасья'])

const diff = names1.difference(names2)

console.log(diff)
// Set(3) { 'Аглая', 'Елизавета', 'Соня' }
```

## `intersection()` сравнивает текущую коллекцию с другой и возвращает новую, состоящую из элементов, входящих в обе коллекции
```js
const num1 = new Set([42, 4, 69, 37, 2])
const num2 = new Set([1, 2, 3, 4])

const inter = num1.intersection(num2)

console.log(inter)
// Set(2) { 2, 4 }
```

## `symmetricDifference()` сравнивает текущую коллекцию с другой и возвращает новую, состоящую из элементов, входящих только в одну из коллекций
```js
const albumList1 = new Set(['White Album', 'Revolver', 'Help!'])
const albumList2 = new Set(['Revolver', 'Rubber Soul', 'Help!', 'Abbey Road'])

const result = albumList1.symmetricDifference(albumList2)

console.log(result)
// Set(3) { 'White Album', 'Rubber Soul', 'Abbey Road' }
```

## `isSubsetOf()` сравнивает текущую коллекцию с другой и возвращает `true`, если все элементы указанной коллекции находятся так же в другой коллекции, и `false` — если нет.
```js
const array1 = [ 34, 42, 0, -8 ]
const array2 = [ -8, 0, 1, 2, 16, 34, 42 ]

const set1 = new Set(array1)
const set2 = new Set(array2)

console.log(set1.isSubsetOf(set2)) // true
console.log(set2.isSubsetOf(set1)) // false
```

## `isSupersetOf()` сравнивает текущую коллекцию с другой и возвращает `true`, если текущая коллекция включает в себя все элементы другой коллекции, и `false` — если нет.
```js
const booksOfSonya = [ 'Дар', 'Подвиг', 'Защита Лужина', 'Отчаяние' ]
const booksOfNadya = [ 'Подвиг', 'Защита Лужина', 'Дар' ]

const set1 = new Set(booksOfSonya)
const set2 = new Set(booksOfNadya)

console.log(set1.isSupersetOf(set2))
// true
console.log(set2.isSupersetOf(set1))
// false
```

## `isDisjointFrom()` позволяет проверить, имеют ли два множества хотя бы один общий элемент. Возвращает `true`, если множества не имеют общих элементов, и `false`, если хотя бы один элемент совпадает
```js
const annaSkills = new Set(['JavaScript', 'HTML', 'CSS', 'Vue.js']);
const pavelSkills = new Set(['Python', 'Node.js', 'PostgreSQL', 'Redis']);

console.log(annaSkills.isDisjointFrom(pavelSkills));
// true, т.к. у Ани и Павла нет общих навыков
```