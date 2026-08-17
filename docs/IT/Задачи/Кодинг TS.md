## Написать типизацию
```ts
const X = { a: 1, b: 2, c: 3, d: 4 };

function getProperty(obj, key) {
    return obj[key];
}

getProperty(X, 'a');
getProperty(X, 'm');
```
> [!faq]- Ответ
> ```ts
> function getProperty<T, K extends keyof T>(obj: T, key: K) {
> 	return obj[key];
> }
> ```

## Реализовать кастомный утилитарный тип Partial
```ts
interface User {
    id: number;
    name: string;
    email: string;
}

type MyPartial<Type> = {
    // Реализовать логику
}

const user: MyPartial<User> = {
    name: 'John'
}
```
> [!faq]- Ответ
> ```ts
> type MyPartial<Type> = {
>     [Key in keyof Type]?: Type[Key];
> }
> ```
## Реализовать кастомный утилитарный тип Omit
> [!faq]- Ответ
> ```ts
> type MyOmit<T, K extends keyof T> = {
>     [Key in keyof T as Key extends K ? never : Key]: T[Key]
> }
> ```

## Реализовать кастомный утилитарный тип Pick
> [!faq]- Ответ
> ```ts
> type MyPick<T, K extends keyof T> = {
>     [Key in K]: T[Key];
> }
> ```

## Реализовать функцию debounce
```ts
// Должно быть выведено только два раза
function onResize() {
    console.log('Resize!');
}

const debounceResize = debounce(onResize, 500);
debounceResize();
debounceResize();
debounceResize();
debounceResize();
debounceResize();
debounceResize();

setTimeout(() => debounceResize(), 1000);
```
> [!faq]- Ответ
> ```ts
> function debounce(callback: Function, delay: number) {
>     let timeout: ReturnType<typeof setTimeout> | undefined;
> 
>     return function(...args: unknown[]) {
>         clearTimeout(timeout);
>         timeout = setTimeout(() => { callback(...args) }, delay);
>     }
> }
> ```

## Реализовать функцию throttle
```ts
function onResize(...args: unknown[]): void {
    console.log('Resized!', ...args);
}

const throttledResize = throttle(onResize, 500);

setTimeout(() => {throttledResize(1, 2, 3)}, 250);
setTimeout(() => {throttledResize(2)}, 500);
setTimeout(() => {throttledResize(3)}, 750);
setTimeout(() => {throttledResize(4)}, 1000);
setTimeout(() => {throttledResize(5)}, 1500);
setTimeout(() => {throttledResize(6)}, 2000);
setTimeout(() => {throttledResize(7)}, 2200);
setTimeout(() => {throttledResize(8)}, 2300);
setTimeout(() => {throttledResize(9)}, 2400);
setTimeout(() => {throttledResize(10)}, 2600);
```
> [!faq]- Ответ
> ```ts
> function throttle(callback: Function, period: number) {
>     let timeout: ReturnType<typeof setTimeout> | undefined;
> 
>     return function(...args: unknown[]) {
>         if (timeout) { return; }
> 
>         timeout = setTimeout(() => {
>             callback(...args)
>             timeout = undefined;
>         }, period);
>     }
> }
> ```
