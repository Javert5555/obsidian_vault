# Express
Express — это веб-инфраструктура маршрутизации и промежуточного программного обеспечения, которая сама по себе имеет минимальную функциональность: приложение Express — это, по сути, серия вызовов функций промежуточного программного обеспечения.

**Аутентификация** — процедура проверки подлинности, например проверка подлинности пользователя путем сравнения введенного им пароля с паролем, сохраненным в базе данных.
**Авторизация** — предоставление определенному лицу или группе лиц прав на выполнение определенных действий.

## use()
**app**.**use**() используется для монтирования функции промежуточного программного обеспечения(middleware) или монтирования по указанному пути, функция промежуточного программного обеспечения выполняется, когда соответствует базовый путь. В приведенном выше коде **app**.

## Middleware
Функции промежуточного ПО (Middleware) — это функции, которые имеют доступ к объекту запроса (req), объекту ответа (res) и следующей функции промежуточного ПО в цикле запроса-ответа приложения. Следующая функция промежуточного программного обеспечения обычно обозначается переменной с именем next.

## Router
**Router** можно создавать модульные, монтируемые обработчики маршрутов. Экземпляр **Router** представляет собой комплексную систему промежуточных обработчиков и маршрутизации; по этой причине его часто называют “мини-приложением”.

### router.param

`router.param(name, callback)`: добавляет триггеры обратного вызова к параметрам маршрута, где name — это имя параметра, а callback — функция обратного вызова.

Например, когда `:user` присутствует в пути маршрута, вы можете сопоставить логику загрузки пользователей, чтобы автоматически предоставлять req.user маршруту или выполнять проверки входных параметров.

```javascript
router.param('user', function (req, res, next, id) {
  // пытаемся получить информацию о пользователе из модели User и прикрепить ее к объекту запроса
  User.find(id, function (err, user) {
    if (err) {
      next(err)
    } else if (user) {
      req.user = user
      next()
    } else {
      next(new Error('failed to load user'))
    }
  })
})
```

Обратный вызов param будет вызываться только один раз в цикле запрос-ответ, даже если параметр соответствует нескольким маршрутам, как показано в следующих примерах.

```javascript
router.param('id', function (req, res, next, id) {
  console.log('CALLED ONLY ONCE')
  next()
})

router.get('/user/:id', function (req, res, next) {
  console.log('although this matches')
  next()
})

router.get('/user/:id', function (req, res) {
  console.log('and this matches too')
  res.end()
})


//CALLED ONLY ONCE
//although this matches
//and this matches too

```
### req.profile
https://stackoverflow.com/questions/29586463/nodejs-express-profile-property-in-request

## Express configuration
1. `body-parser`: промежуточное программное обеспечение (middleware) для разбора тела запроса для обработки сложности разбора потоковых объектов запроса, чтобы мы могли упростить взаимодействие между браузером и сервером путем обмена JSON в теле запроса. Чтобы установить модуль, запустите `npm i body-parser` из командной строки. Затем настройте приложение Express с bodyParser.json(), bodyParser.urlencoded({ extended: true }).
2. `cookie-parser`: промежуточное программное обеспечение (middleware) для разбора и установки файлов cookie в объектах запросов. Чтобы установить модуль cookie-parser, выполните команду `npm i cookie-parser` из командной строки.
3. `compression`: модуль сжатия, который будет пытаться сжимать тела ответов для всех запросов, проходящих через модуль. Чтобы установить модуль сжатия, выполните команду `npm i compression` из командной строки.
4. `helmet`: набор функций промежуточного ПО (middleware) для обеспечения безопасности приложений Express путем установки различных HTTP-заголовков. Чтобы установить модуль helmet, выполните команду `npm i helmet` из командной строки.
5. `cors`: промежуточное программное обеспечение (middleware) для обеспечения кросс-оригинального обмена ресурсами (CORS). CORS — механизм, использующий дополнительные HTTP-заголовки, чтобы дать возможность агенту пользователя получать разрешения на доступ к выбранным ресурсам с сервера на источнике (домене), отличном от того, что сайт использует в данный момент. Чтобы установить модуль cors, выполните команду `npm i cors` из командной строки.

## req

Всякий раз, когда сервер приложений Express получает HTTP-запрос, он предоставляет разработчику объект, обычно называемый res. Например,

``` javascript
app.get('/test', (req, res) => {
   // use req and res here
})
```

Объект res в основном относится к ответу, который будет отправлен как часть этого вызова API.  
  
Функция res.send устанавливает тип содержимого в text/Html, что означает, что клиент теперь будет обрабатывать его как текст. Затем он возвращает ответ клиенту.  
  
Функция res.json на других телефонах устанавливает заголовок типа содержимого в application/JSON, чтобы клиент обрабатывал строку ответа как допустимый объект JSON. Затем он также возвращает ответ клиенту.