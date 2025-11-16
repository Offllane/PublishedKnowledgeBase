### Определение
Программные сущности (классы, компоненты, модули, функции) должны быть открыты для расширения, но закрыты для изменения.

### Какие проблемы решает этот принцип
Изменять существующий программный код плохо, т.к. он уже протестирован, работает и у нас с ним проблем нет. Если мы изменяем существующий код, то мы должны проводить регрессионное тестирование и проверять, что мы не сломали существующий функционал.

Нужно стараться добавлять новый функционал не за счет изменения новых сущностей, а за счет добавления новой сущности и реализовывать новый функционал уже там.

Естественно этим правилом не получится пользоваться в 100 процентах случаев, т.к. есть багфиксы, исправления логики и т.п. и нам в любом случае придется изменять старый код.

### Как работает принцип
#### Появление проблемы
У нас есть класс Оружие. У него есть три поля: тип, урон, дистанция. 
```ts
class Weapon {  
  type: string;  
  damage: number;  
  range: number;  
  
  constructor(type: string, damage: number, range: number) {  
    this.type = type;  
    this.damage = damage;  
    this.range = range;  
  }}
```

И есть класс Персонаж, который это Оружие может принимать.
```ts
class Character {  
  name: string;  
  weapon: Weapon;  
  
  constructor(name: string, weapon: Weapon) {  
    this.name = name;  
    this.weapon = weapon;  
  }  
  changeWeapon(newWeapon: Weapon): void {  
    this.weapon = newWeapon;  
  }}
```

Создаем оружие Меч с конкретными параметрами дистанции и урона. Создаем персонажа и даем ему Меч.
```ts
const sword = new Weapon('sword', 15, 2);  
  
const character = new Character('Warrior', sword);
```

После чего в классе оружия создаем метод Атака.
```ts
class Weapon {  
  /.../  
  attack(): void {  
    console.log('Удар мечом с уроном ' + this.damage);  
  }}
```

Персонажу мы делегируем метод Атака из класса Оружие, после чего этот метод вызываем уже у персонажа.
```ts
class Character {  
 /.../  
 attack() {  
   this.weapon.attack();  
 }}

 /.../ 
 character.attack();
```

Получаем ожидаемый результат
![[Pasted image 20240804222235.png]]

Теперь нужно добавить новый функционал -- арбалет.
Создадим новое Оружие с типом Арбалет и поменяем оружие Меч на оружие Арбалет. После этого атакуем. 
```ts
/.../ 
const crossbow = new Weapon('crossbow', 40, 70);  

character.changeWeapon(crossbow);
character.attack();
```

Получаем тоже вполне ожидаемый результат: два раза удар мечом с разным уроном.
![[Pasted image 20240804222637.png]]

И вот на данном этапе мы идем в уже протестированный и рабочий код, и изменяем его.
```ts
class Weapon {  
   /.../ 
  attack(): void {  
    switch (this.type) {  
      case 'sword': {  
        console.log('Удар мечом с уроном ' + this.damage);  
        return;  
      }     
      case 'crossbow': {  
        console.log('Выстрел из арбалета с уроном ' + this.damage);  
        return;  
      }    
    }  
  }
}
```

Да, результат мы получаем, но не за счет добавления новой сущности, а за счет изменения уже существующей.

#### Решение проблемы
Вынесем метод attack в интерфейс Attacker, чтобы мы могли его переиспользовать в других классах.
```ts
interface Attacker {  
  attack: () => void;  
}  
  
class Weapon implements Attacker {  
  /.../ 
  attack(): void {}  
}
```
 
Затем мы создаем класс Меч, который наследуется от класса Оружие.
Затем создаем класс Арбалет таким же образом, переопределяя конкретный класс.
```ts
class Sword extends Weapon {  
  override attack() {  
    console.log('Удар мечом с уроном ' + this.damage);  
  }
}  
  
class Crossbow extends Weapon {  
  override attack() {  
    console.log('Выстрел из арбалета с уроном ' + this.damage);  
  }
}
```

Теперь мы можем избавиться от свойства "тип" в классе Оружие.
```ts
class Weapon implements Attacker {  
  // type: string;  
  damage: number;  
  range: number;  
  
  constructor(// type: string,
			   damage: number, range: number) {  
    // this.type = type;  
    this.damage = damage;  
    this.range = range;  
  }  
  attack(): void {}  
}
```

Теперь, чтобы создать новый тип оружия, нам достаточно создать новый класс, унаследовать его от базового класса Оружие, и новый функционал у нас добавлен.
```ts
const sword = new Sword(15, 2);  
const character = new Character('Warrior', sword);  
character.attack();  
  
const crossbow = new Crossbow(40, 70);  
character.changeWeapon(crossbow);  
character.attack();
```

Теперь все работает как надо, на нарушая принцип открытости-закрытости.
![[Pasted image 20240804231900.png]]

Т.е. когда вы продумываете схему ваших сущностей стоит подумать заранее о том, что вам придется добавить новый функционал. И что этот функционал лучше будет добавлять не путем накостыливания дополнительный условий, а путем расширения.

Теперь если мы захотим добавить в приложение Нож, то нам не нужно будет дополнительно тестировать Меч и Арбалет, т.к. мы не вносим никаких изменений в существующий код. Мы добавим Нож и протестируем только его.
```ts
class Knife extends Weapon {  
  override attack() {  
    console.log('Удар ножом с уроном ' + this.damage);  
  }}
```
