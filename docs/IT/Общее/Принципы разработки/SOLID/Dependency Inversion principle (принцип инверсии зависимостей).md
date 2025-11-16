### Определение
Модули высокого уровня не должны зависеть от модулей более низкого уровня. Все они должны зависеть от абстракций. Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.
### Какие проблемы решает принцип
В случае нарушения этого принципа, нам руками приходится менять сущности (классы и модули) при изменении модулей более низкого уровня.
### Как работает принцип
![[Pasted image 20251116213917.png]]
Представим, что у нас есть завод с работниками и станками. В станках есть детали.
Предположим, что какая делать в станке сломалась и мы ее заменили. И теперь с новой деталью логика работы станка изменилась и работники уже не могут работать на этом станке. Более того, для этой детали нужно другое напряжение для работы.
Это как раз пример нарушения принципа инверсии зависимости -- у нас модули более высокого уровня зависят от модулей более низкого уровня.

> Из-за одной детали пришлось заменить работников, которые могут работать на этом станке и пришлось переделывать электричество (мощность, напряжение и т.п.)

#### Появление проблемы
Разрабатываем приложение для прослушивания музыки. Решаем, что получать треки мы будем с помощью Яндекс Апи.
Мы создаем экземляр класса Яндекс АПИ и начинаем с этим экземпляром работать во всем приложении.
```ts
class YandexMusicAPI {  
  public get() {}  
}  
  
const MusicApp = () => {  
  const API = new YandexMusicAPI();  
  
  API.get();  
}
```

Потом мы решаем, что будем работать со спотифай АПИ тоже. ООП мы не знаем, общую функциональность в интерфейсы не выносим и получается вот так:
```ts
class YandexMusicAPI {  
  public get() {}  
}  
  
class SpotifyAPI {  
  public fetchAll() {};  
}  
  
const MusicApp = () => {  
  const API = new SpotifyAPI(); // тут поменяли  
  
  API.fetchAll(); // и тут пришлось менять  
}
```

Потом добавляется еще VKMusic и получается каша
```ts
class YandexMusicAPI {  
  public get() {}  
}  
  
class SpotifyAPI {  
  public fetchAll() {}  
}  
  
class VKMusicAPI {  
  public query() {}  
}
```
#### Решение проблемы
Мы решаем создать общий интерфейс, описываем там метод getTracks(),  имплементируем его в каждый класс и уже ситуация становится получше. Мы хотя бы имеем одинаковые названия методов.
```ts
interface MusicAPI {  
  getTracks: () => Track;  
}  
  
class YandexMusicAPI implements MusicAPI {  
  public getTracks() {}  
}  
  
class SpotifyAPI implements MusicAPI {  
  public getTracks() {}  
}  
  
class VKMusicAPI implements MusicAPI {  
  public getTracks() {}  
}
```

Но представим, что у нас в 100 разных файлах используется SpotifyAPI, а мы решили использоваться YandexMusicAPI -- все придется менять руками. Да, IDE может при этом помочь, но это неправильный путь.
```ts
const MusicApp = () => {  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  const API: MusicAPI = new SpotifyAPI();  
  
  API.getTracks();  
}
```

Было бы куда грамотнее создать абстракцию -- звено между клиентом и различными видами АПИ для получения музыки.
```ts
class MusicClient implements MusicAPI {  
  public client: MusicAPI;  
  
  constructor(client: MusicAPI) { // получаем нужный клиент  
    this.client = client;  
  }  
  
  public getTracks() {  
    this.client.getTracks(); // делегируем вызов метода  
  }  
}  
  
const MusicApp = () => {  
  const API: MusicAPI = new MusicClient(new SpotifyAPI());  
  
  API.getTracks();  
}
```
И на данном этапе мы все взаимодействие с АПИ ведем через этот клиент. И теперь нам абсолютно все равно, получаем ли мы музыку из Яндекса или из ВК. В любой момент мы можем это поменять, передав нужный класс. И при этом это нужно будет сделать только в одном месте.