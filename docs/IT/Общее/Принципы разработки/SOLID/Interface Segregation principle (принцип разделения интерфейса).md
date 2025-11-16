### Определение
Программные сущности не должны зависеть от методов, которые они не используют.

Нельзя заставлять клиента реализовывать интерфейс, которым он не пользуется.

Неправильная реализация
![[Pasted image 20251116172016.png]]

Правильная реализация
![[Pasted image 20251116172056.png]]
### Какие проблемы решает принцип
1. Избавляем программные сущности от методов, которые они не используют.
2. Получаем более предсказуемую работу.
3. Код становится менее связанным.

### Как работает принцип
>При проектировании интерфейсы нужно продумывать так, чтобы они подходили под конкретного клиента. Делать общее решение -- антипаттерн.
#### Появление проблемы
Есть интерфейс Weapon в котором есть метод attack(). Создаем класс Пистолет, который имплементирует этот метод.

```js
interface Weapon {  
  attack: () => void;  
}  
  
class Pistolet implements Weapon {  
  public attack(): void { /.../ }  
}
```

После этого мы понимаем, что Пистолет нужно еще перезаряжать. Для этого добавляем ему метод reload() и добавляем этот метод в интерфейс.
```js
interface Weapon {  
  attack: () => void;  
  reload: () => void;  
}  
  
class Pistolet implements Weapon {  
  public attack(): void { /.../ }  
  public reload(): void { /.../ }  
}
```

Затем добавляем в класс RPG и реализуем методы. И в данный момент у нас тоже все отлично.
```js
class RPG implements Weapon {  
  public attack(): void { /.../ }  
  public reload(): void { /.../ }  
}
```

А потом нам становится необходимо добавить класс Нож. И тут появляется проблема -- у ножа не должно быть метода reload(). 
```js   
class Knife implements Weapon {  
  public attack(): void { /.../ }  
  public reload(): void { return; } // а вот этого метода у ножа быть не должно  
}
```
#### Решение проблемы
Если следовать принципу разделения интерфейсов, то было бы логичным разделить исходный интерфейс, на два.

Реализация должна выглядеть вот так:
```js
interface Attacker {  
  attack: () => void;  
}  
  
interface Reloader {  
  reload: () => void;  
}  
  
class Pistolet implements Attacker, Reloader {  
  public attack(): void { /.../ }  
  public reload(): void { /.../ }  
}  
  
class RPG implements Attacker, Reloader {  
  public attack(): void { /.../ }  
  public reload(): void { /.../ }  
}  
  
class Knife implements Attacker {  
  public attack(): void { /.../ }  
}
```