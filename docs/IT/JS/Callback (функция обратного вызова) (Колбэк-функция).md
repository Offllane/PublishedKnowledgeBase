Колбэк-функция (или обратный вызов) - это функция, переданная в другую функцию в качестве аргумента, которая затем вызывается по завершению какого-либо действия.

## Синхронный колбэк
```js
function greetings(name) {  
    alert(`Hello ${name}`);  
}  

function getUserName() {  
    return prompt('Enter user name');  
}  
  
function processUserName(callback) {  
    const name = getUserName() ?? 'unknown user';  
    callback(name);  
}
  
processUserName(greetings);
```
Функция `greetings` передается в качестве аргумента (как обычная переменная) в функцию `processUserName` и будет вызвана позже в функции как `имя_функции()`. Это и называется колбэк.

## Асинхронный колбэк
Колбэки часто используются для продолжения выполнения кода после завершения асинхронной операции - они называются асинхронными колбэками.
```js
async function getPosts() {  
    try {  
        const posts = await fetch('https://jsonplaceholder.typicode.com/posts');  
        return posts.json();  
    } catch (error) {  
       // error handling  
    }  
}  
  
function handlePostsLoad(posts) {  
    console.log(posts);  
}  
  
async function loadPage(callback) {  
    const posts = await getPosts();  
    callback(posts);  
}  
  
loadPage(handlePostsLoad);
```
при этом колбэк может быть (и чаще всего бывает) [[Arrow functions (Стрелочные функции)|стрелочной функцией]]. 

```js
function handlePostsLoad(posts) {  
    console.log(posts);  
} 
loadPage(handlePostsLoad);

// идентично
loadPage((posts) => {  
    console.log(posts);  
});
```

Но вообще пример не очень удачный, скорее всего это будет записано вот так:
```js
async function loadPage() {  
    return await getPosts();  
}  
  
loadPage().then((posts) => {  
    console.log(posts);  // идентично предыдущему примеру
});
```

## Колбэки в методах массива
Часто при работе с массивами есть необходимость передать в метод колбэк-функцию.
```js
const persons = [  
    { name: 'Egor', age: 24, budget: 40000 },  
    { name: 'Vitaliy', age: 25, budget: 20000 },  
    { name: 'Sasha', age: 34, budget: 18000 },  
    { name: 'Maksim', age: 20, budget: 10000 },  
    { name: 'Andrey', age: 52, budget: 10000 },  
    { name: 'Dima', age: 31, budget: 25000 },  
    { name: 'Artem', age: 18, budget: 17000 },  
]  
  
function logName(person) {  
    console.log(person.name);  
}  
  
persons.forEach(logName);

// or

persons.forEach((person) => {  
    console.log(person.name)  
});
```

```js
const persons = [  
    { name: 'Egor', age: 24, budget: 40000 },  
    { name: 'Vitaliy', age: 25, budget: 21000 },  
    { name: 'Sasha', age: 34, budget: 18000 },  
    { name: 'Maksim', age: 20, budget: 10000 },  
    { name: 'Andrey', age: 52, budget: 10000 },  
    { name: 'Dima', age: 31, budget: 25000 },  
    { name: 'Artem', age: 18, budget: 17000 },  
]  
  
function isYoungTalent(person) {  
    const { age, budget } = person;  
    return age < 30 && budget > 20000;  
}  
  
const youngTalents = persons.filter(isYoungTalent);  

// or

const youngTalents = persons.filter((person) => {  
    const { age, budget } = person;  
    return age < 30 && budget > 20000;  
});

console.log(youngTalents);
```