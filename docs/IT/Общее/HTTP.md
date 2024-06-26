### Что такое HTTP?
**HTTP (HyperText Transfer Protocol — «протокол передачи гипертекста»)** — это протокол, позволяющий получать различные ресурсы, например HTML-документы. Протокол HTTP лежит в основе обмена данными в Интернете. HTTP является протоколом клиент-серверного взаимодействия, что означает инициирование запросов к серверу самим получателем, обычно веб-браузером (web-browser).

HTTP представляет собой расширяемый протокол прикладного уровня, на котором работает весь веб-сегмент интернета.
### Основные свойства HTTP
- версии HTTP/1.1 – действующий (текстовый), HTTP/2 – черновой (не распространен, бинарный);
- два типа абонентов: клиент и сервер;
- два типа сообщений: request и response;
- от клиента к серверу – request;
- от сервера к клиенту – response;
- на один request всегда один response, иначе ошибка;
- одному response всегда один request,  иначе ошибка;
- TCP-порты: 80, 443;
- для адресации используется URI или URN.
### URL
Получение доступа к ресурсам по HTTP-протоколу осуществляется с помощью указателя URL (Uniform Resource Locator). URL представляет собой строку, которая позволяет указать запрашиваемый ресурс и еще ряд параметров.

Использование URL неразрывно связано с другими элементами протокола, поэтому далее мы рассмотрим его основные компоненты и строение:
#### Scheme
Поле **Scheme** используется для указания используемого протокола, всегда сопровождается двоеточием и двумя косыми чертами (://).
#### Host
**Host** указывает местоположение ресурса, в нем может быть как доменное имя, так и IP-адрес.
#### Port
**Port**, как можно догадаться, позволяет указать номер порта, по которому следует обратиться к серверу. Оно начинается с двоеточия (:), за которым следует номер порта. При отсутствии данного элемента номер порта будет выбран по умолчанию в соответствии с указанным значением **Scheme** (например, для http:// это будет порт 80).
#### Path
Далее следует поле **Path**. Оно указывает на ресурс, к которому производится обращение. Если данное поле не указано, то сервер в большинстве случаев вернет указатель по умолчанию (например index.html).
#### Query String
Поле **Query String** начинается со знака вопроса (?), за которым следует пара «параметр-значение», между которыми расположен символ равно (=). В поле Query String могут быть переданы несколько параметров с помощью символа амперсанд (&) в качестве разделителя.

Не все компоненты необходимы для доступа к ресурсу. Обязательно следует указать только поля **Scheme** и **Host**.
### Request (из чего состоит)
- метод;
- URI;
- версия протокола (HTTP/1.1);
- заголовки (пары: имя/заголовок);
- параметры (пары: имя/заголовок);
- тело запроса (опционально).
![[Pasted image 20240420131518.png]]

### Response (из чего состоит)
- версия протокола (HTTP/1.1);
- код состояния (1xx, 2xx, 3xx, 4xx, 5xx);
- пояснение к коду состояния;
- заголовки (пары: имя/заголовок);
- тело ответа (опционально).
![[Pasted image 20240420131634.png]]

### Request методы
- GET;
- POST;
- PUT;
- DELETE;
- OPTIONS;
- HEAD;
- TRACE;
- CONNECT

#### GET
Позволяет запросить некоторый конкретный ресурс. Дополнительные данные могут быть переданы через строку запроса (Query String) в составе URL (например ?param=value);
#### POST
Позволяет отправить данные на сервер. Поддерживает отправку различных типов файлов, среди которых текст, PDF-документы и другие типы данных в двоичном виде. Обычно метод POST используется при отправке информации (например, заполненной формы логина) и загрузке данных на веб-сайт, таких как изображения и документы.
#### PUT
Используется для создания (размещения) новых ресурсов на сервере. Если на сервере данный метод разрешен без надлежащего контроля, то это может привести к серьезным проблемам безопасности.
#### DELETE
Позволяет удалить существующие ресурсы на сервере. Если использование данного метода настроено некорректно, то это может привести к атаке типа «Отказ в обслуживании» (Denial of Service, DoS) из-за удаления критически важных файлов сервера.
#### HEAD
Обычно сервер в ответ на запрос присылает в ответе тело и заголовки.
Данный метод при использовании его в запросе позволит получить только заголовки, которые сервер бы вернул при получении GET-запроса к тому же ресурсу. Запрос с использованием данного метода обычно производится для того, чтобы узнать размер запрашиваемого ресурса перед его загрузкой.
#### PATCH
Позволяет внести частичные изменения в указанный ресурс по указанному расположению.

### Заголовки
**HTTP-заголовок** представляет собой строку формата «Имя-Заголовок:Значение», с двоеточием(:) в качестве разделителя. Название заголовка не учитывает регистр, то есть между Host и host, с точки зрения HTTP, нет никакой разницы. Однако в названиях заголовков принято начинать каждое новое слово с заглавной буквы. Структура значения зависит от конкретного заголовка. Несмотря на то, что заголовок вместе со значениями может быть достаточно длинным, занимает он всего одну строчку.
#### General
Общие заголовки, используются в запросах и ответах
![[Pasted image 20240420133140.png]]
#### Request
Используются только в запросах
![[Pasted image 20240420133214.png]]
#### Response
Используются только в ответах
![[Pasted image 20240420133305.png]]
#### Entity
Для сущности в ответах и запросах
![[Pasted image 20240420133358.png]]

### Примеры заголовков запроса
#### Host
Используется для указания того, с какого конкретно хоста запрашивается ресурс. В качестве возможных значений могут использоваться как доменные имена, так и IP-адреса. На одном HTTP-сервере может быть размещено несколько различных веб-сайтов. Для обращения к какому-то конкретному требуется данный заголовок.
```
Host: <host>:<port>

Host: developer.mozilla.org
```
#### User-Agent
Заголовок используется для описания клиента, который запрашивает ресурс. Он содержит достаточно много информации о пользовательском окружении. Например, может указать, какой браузер используется в качестве клиента, его версию, а также операционную систему, на которой этот клиент работает.
```
User-Agent: Mozilla/5.0 (<system-information>) <platform> (<platform-details>) <extensions>
```
#### Refer
Используется для указания того, откуда поступил текущий запрос. Например, если вы решите перейти по какой-нибудь ссылке в этой статье, то вероятнее всего к запросу будет добавлен заголовок Refer: https://selectel.ru
#### Accept
Позволяет указать, какой тип медиафайлов принимает клиент. В данном заголовке могут быть указаны несколько типов, перечисленные через запятую (‘ , ‘). А для указания того, что клиент принимает любые типы, используется следующая последовательность — `*/*`.
```
Accept: <MIME_type>/<MIME_subtype>
Accept: <MIME_type>/*
Accept: */*

// Несколько типов, дополненных синтаксисом значений качества (en-US):
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8

// example
Accept: text/html
Accept: image/*
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8
```
#### Cookie
Данный заголовок может содержать в себе одну или несколько пар «Куки-Значение» в формате cookie=value. Куки представляют собой небольшие фрагменты данных, которые хранятся как на стороне клиента, так и на сервере, и выступают в качестве идентификатора. Куки передаются вместе с запросом для поддержания доступа клиента к ресурсу. Помимо этого, куки могут использоваться и для других целей, таких как хранение пользовательских предпочтений на сайте и отслеживание клиентской сессии. Несколько кук в одном заголовке могут быть перечислены с помощью символа точка с запятой (‘ ; ‘), который  используется как разделитель.
```
Cookie: <cookie-list>
Cookie: name=value
Cookie: name=value; name2=value2; name3=value3

// example
Cookie: PHPSESSID=298zf09hf012fh2; csrftoken=u32t4o3tb3gg43; _gat=1
```
#### Authorization
Используется в качестве еще одного метода идентификации клиента на сервере. После успешной идентификации сервер возвращает токен, уникальный для каждого конкретного клиента. В отличие от куки, данный токен хранится исключительно на стороне клиента и отправляется клиентом только по запросу сервера. Существует несколько типов аутентификации, конкретный метод определяется тем веб-сервером или веб-приложением, к которому клиент обращается за ресурсом.
```
Authorization: <тип> <данные пользователя>

Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
```
### Примеры заголовков ответа
#### Server
Содержит информацию о сервере, который обработал запрос.
```
Server: ngnix
```
#### Set-Cookie
Содержит куки, требуемые для идентификации клиента. Браузер парсит куки и сохраняет их в своем хранилище для дальнейших запросов.
```
Set-Cookie:PHPSSID=bf42938f
```
#### WWW-Authenticate
Уведомляет клиента о типе аутентификации, который необходим для доступа к запрашиваемому ресурсу.
```
WWW-Authenticate: BASIC realm=»localhost
```
### Коды состояний response
- 1xx: информационные сообщения;
- 2xx: успешный ответ;
- 3xx: переадресация;
- 4xx: ошибка клиента;
- 5xx: ошибка сервера.
![[Pasted image 20240420133906.png]]



