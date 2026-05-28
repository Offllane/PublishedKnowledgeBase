Сигнал - это обертка вокруг значения, которая уведомления всех заинтересованных потребителей этого значения об изменениях.

```ts
protected user: WritableSignal<User> = signal<User>({  
  name: 'Ivan',  
  surname: 'Petrov'  
});  
  
protected userFullName: Signal<string> = computed(() => this.computeFullName()));
  
protected updateUser(): void {  
  this.user.update((user: User) => ({  
    ...user,  
    surname: 'Ivanov'  
  }));  
}  
  
constructor() {  
  effect(() => {  
    console.log('user was updated', this.user());  
  });  
}

protected computeFullName(): string {  
  return `${this.user().name}, ${this.user().surname}`  
}
```

>[!warning] Мутация не триггерит обновление
```ts
protected updateUser(): void {  
  this.user().name = 'Sasha';   // нужно либо через .update(), либо через .set()
}  
  
protected userFullName: Signal<string> = computed(() => {  // не вызовется
  return `${this.user().name}, ${this.user().surname}`  
});  
  
constructor() {  
  effect(() => {  
    console.log('user was updated', this.user()); // не вызовется
  });  
}
```

```ts
effect(() => {
  console.log(`User set to ${currentUser()} and the counter is ${untracked(counter)}`);
}); // будет выводиться только если изменился currentUser()
```

Синтаксис: https://www.youtube.com/watch?v=kXtpXObWekY&t=1028s
Как работают под капотом: https://www.youtube.com/watch?v=6pQAPjtg1jM

>[!note] Полезное
> 1. `computed()` вычисляется только в момент, когда мы обращаемся к значению `computed()`.
>2. Если значение `computed()` нигде не задействовано, то расчет проводится не будет. 
>3. Зависимости `computed()` могут динамически изменяться. (В зависимости от условий других сигналов)
>4. Используем `untracked(signalName())`, чтобы читать значение сигнала, но не создавать зависимость
>5. Можно передавать функцию сравнения




