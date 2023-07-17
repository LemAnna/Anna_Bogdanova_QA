
Баг-репорты [Fake REST API](https://fakerestapi.azurewebsites.net/index.html)


## Баги
При тестировании Fake REST API нашей командой были найдены следующие баги, которые распространяются на все блоки:
1) Невозможно реально создавать, удалять и изменять сущности. При выполнении данных действий методами POST, DELETE и PUT, сваггер выдает информацию о их успешном выполнении, но последующая проверка методом GET показывает, что указанные действия не были произведены.
2) При создании сущности методом POST выдает код 200 OK вместо 201 Created
3) Fake REST API позволяет вводить отрицательные значения ID в запросах методов PUT, GET и DELETE и осуществлять по ним поиск, а так-же, позволяет вводить отрицательные значения ID в теле запросов PUT и POST 
4) При вызове метода GET с минусовым значением ID, Fake REST API выдает ошибку 404 (не найдено) вместо 400 (плохой запрос) 
5) В некоторых блоках метод GET позволяет вызывать сущности по несуществующим ID. 

#### Окружение: Win10Pro, FF 113.0.1, Windows10Дом22H2, Edge ####

| | | | | | | | |
|-|-|-|-|-|-|-|-|
|ID|Название|Предусловия|Шаги|Ожидаемый результат|Результат шага|Фактический результат|Критичность|
|36|POST запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Activities|Успешен| | |
| | | |Инициировать отправку запроса  методом  POST для добавления сущности с телом: {  "id": 5,  "title": "string",  "dueDate": "2023-05-12T08:08:54.298Z",  "completed": true }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X POST "https://fakerestapi.azurewebsites.net/api/v1/Activities" -H  "accept: text/plain; v=1.0" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":5,\"title\":\"string\",\"dueDate\":\"2023-05-12T08:08:54.298Z\",\"completed\":true}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 201 OK|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 5,  "title": "string",  "dueDate": "2023-05-12T08:08:54.298Z",  "completed": true }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:  "id" - integer,  "title" - string,  "dueDate" - string,  "completed" - boolean|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  access-control-allow-origin: *   api-supported-versions: 1.0   content-type: application/json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=5|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 5,  "title": "string",  "dueDate": "2023-05-12T08:08:54.298Z",  "completed": true }|Провален|{   "id": 5,   "title": "Activity 5",   "dueDate": "2023-05-22T20:09:10.3608852+00:00",   "completed": false }| |
|11| PUT запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |​/api​/v1​/Activities​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для изменения активности с id=1 методом PUT с телом:  {  "id": 2,  "title": "testtitle",  "dueDate": "2023-05-11T16:36:18.488Z",  "completed": false }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X PUT "https://fakerestapi.azurewebsites.net/api/v1/Activities/1" -H  "accept: text/plain; v=1.0" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":2,\"title\":\"testtitle\",\"dueDate\":\"2023-05-11T16:36:18.488Z\",\"completed\":false}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 2,   "title": "testtitle",   "dueDate": "2023-05-11T16:36:18.488Z",   "completed": false }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:  "id" - integer,  "title" - string,  "dueDate" - string,  "completed" - boolean|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=2|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 2,   "title": "testtitle",   "dueDate": "2023-05-11T16:36:18.488Z",   "completed": false }|Провален|{   "id": 2,   "title": "Activity 2",   "dueDate": "2023-05-22T17:10:52.0938543+00:00",   "completed": true }| |
|14|DELETE запрос позитивный (id2)| | | | | |Critical|
| | |Endpoint запроса| |​/api​/v1​/Activities​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=2 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Activities/2" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=2|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-fa1ce0a39a7ed74d9e9dec6e68a08872-fc16e87d792b5043-00" }|Провален|{   "id": 2,   "title": "Activity 2",   "dueDate": "2023-05-22T17:10:52.0938543+00:00",   "completed": true }| |
|58|GET запрос негативный (id-1)| | | | | |Trivial|
| | |Endpoint запроса| |​/api​/v1​/Activities​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Activities/-1" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-727eb5f3f369be4fbdcc3f3efcbc3823-a984b02945ac8742-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-aabd8b4699fa8b4086c5709f58a9b361-880c4d0e825d6544-00" }| |
| | | |Проверить структуру ответа|В ответе должен присутствовать статус ответа и причина возникновения ошибки на стороне клиента:   "title": "Bad Request",   "status": 400,|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|59|GET запрос негативный (id0)| | | | | |Major|
| | |Endpoint запроса| |​/api​/v1​/Activities​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Activities/0" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-727eb5f3f369be4fbdcc3f3efcbc3823-a984b02945ac8742-00",   "errors": {     "id": [       "The value '0' is not valid."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-aabd8b4699fa8b4086c5709f58a9b361-880c4d0e825d6544-00" }| |
| | | |Проверить структуру ответа|В ответе должен присутствовать статус ответа и причина возникновения ошибки на стороне клиента:    "title": "Bad Request",   "status": 400,|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|77| POST невалидные данные: ID, отрицательные значения| | | | | |Major|
| | |Endpoint запроса/api​/v1​/Activities​/,  Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": -2,  "title": "string",  "dueDate": "2023-05-13T13:24:00.685Z",  "completed": true }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки запроса и тело|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-051f77baf8fa9f4b85d7a982b8cf1195-feaea70d7527724e-00",   "errors": {     "$.id": [       "'r' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 9."     ]   } }|Провален|{  "id": -2,  "title": "string",  "dueDate": "2023-05-13T13:24:00.685Z",  "completed": true }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:  "errors": {     "$.id": [       "'-' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 9."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|124| DELETE запрос негативный  (id-1)| | | | | |Major|
| | |Endpoint запроса| |​/api​/v1​/Activities​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=-1 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Activities/-1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|В теле должен быть указан статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-c3f56fc018319546a22da69afd55018e-c0be820284c59945-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *   api-supported-versions: 1.0   content-length: 0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|125|  DELETE запрос негативный  (id0)| | | | | |Major|
| | |Endpoint запроса| |​/api​/v1​/Activities​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=0 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Activities/0" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|В теле должен быть указан статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-c3f56fc018319546a22da69afd55018e-c0be820284c59945-00",   "errors": {     "id": [       "The value '0' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *   api-supported-versions: 1.0   content-length: 0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|138| PUT невалидное значение ID в запросе, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/Activities/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=-1 методом PUT с телом:  {  "id": 123,  "title": "string",  "dueDate": "2023-05-14T10:17:25.976Z",  "completed": true }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-659d9e4b8cf27548871c7b5993867565-84ccb8edd43dba46-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|{   "id": 123,   "title": "string",   "dueDate": "2023-05-22T15:46:48.662Z",   "completed": true }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "id": [       "The value '-1' is not valid."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|144| PUT невалидное значение ID, отрицательное значение | | | | | |Major|
| | |Endpoint запроса /api/v1/Activities/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=21 методом PUT с телом: {  "id": -21,  "title": "string",  "dueDate": "2023-05-14T12:21:07.265Z",  "completed": true }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-938ebb735e92c943957b7f09889bb403-4317a3d3dc2d954e-00",   "errors": {     "$.id": [       "'#' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   } }|Провален|{  "id": -21,  "title": "string",  "dueDate": "2023-05-14T12:21:07.265Z",  "completed": true }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'-' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|15| PUT запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |​​/api​/v1​/Authors​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для изменения активности с id=1 методом PUT с телом:  {  "id": 2,  "idBook": 1,  "firstName": "test",  "lastName": "teststring" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X PUT "https://fakerestapi.azurewebsites.net/api/v1/Authors/1" -H  "accept: text/plain; v=1.0" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":2,\"idBook\":1,\"firstName\":\"test\",\"lastName\":\"teststring\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 2,   "idBook": 1,   "firstName": "test",   "lastName": "teststring" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:  id - integer($int32)  idBook - integer($int32)  firstName - string, nullable: true lastName - string, nullable: true|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date:  server: Kestrel|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=2|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 2,   "idBook": 1,   "firstName": "test",   "lastName": "teststring" }|Провален|{   "id": 2,   "idBook": 1,   "firstName": "First Name 2",   "lastName": "Last Name 2" }| |
|17|DELETE запрос позитивный (id2)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Authors/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=2 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Authors/2" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date:  server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной сущности методом GET по ее ID=2|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d643f417984b464d8450b50a851e5023-ae5d68c258ad1047-00" }|Провален|{   "id": 2,   "idBook": 1,   "firstName": "First Name 2",   "lastName": "Last Name 2" }| |
|42|POST запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Authors|Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": 0,  "idBook": 0,  "firstName": "string",  "lastName": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X POST "https://fakerestapi.azurewebsites.net/api/v1/Authors" -H  "accept: text/plain; v=1.0" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":0,\"idBook\":0,\"firstName\":\"string\",\"lastName\":\"string\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 201 OK|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|{   "id": 0,   "idBook": 0,   "firstName": "string",   "lastName": "string" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:  id - integer($int32), idBook - integer($int32)  firstName - string, nullable: true lastName - string, nullable: true|Успешен| | |
| | | |Проверить заголовки ответа|api-supported-versions: 1.0 content-type: application/json; charset=utf-8; v=1.0  date: server: Kestrel transfer-encoding: chunked|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=0|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 0,   "idBook": 0,   "firstName": "string",   "lastName": "string" }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-bee82ed6a19bbf4985804c6cd59494c5-49604c2782f28849-00" }| |
|61| GET{idBook}  запрос негативный (id-1)| | | | | |Trivial|
| | |Endpoint запроса| |/api/v1/Authors/authors/books/{idBook}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с idBook=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Authors/authors/books/-1" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-18fee6b707b2614c89aca034dd7680e8-fb0caabf62131448-00",   "errors": {     "idBook": [       "The value '-1' is not valid."     ]   } }|Провален|[]| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента:   "errors": {     "idBook": [       "The value '-1' is not valid."     ]   } }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|62| GET{idBook}  запрос негативный (id0)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Authors/authors/books/{idBook}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с idBook=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Authors/authors/books/0" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-18fee6b707b2614c89aca034dd7680e8-fb0caabf62131448-00",   "errors": {     "idBook": [       "The value '0' is not valid."     ]   } }|Провален|[]| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента:   "errors": {     "idBook": [       "The value '0' is not valid."     ]   } }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|63|  GET{idBook}  запрос негативный (id201)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Authors/authors/books/{idBook}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с idBook=201 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Authors/authors/books/201" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-9821eba73a6af34492712f9fc4ec6924-dcabba8ec096ce4b-00" }|Провален|[]| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента:   "errors": {     "idBook": [       "The value '201' is not found"     ]   } }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|82| POST невалидные значение: ID, отрицательные значения| | | | | |Major|
| | |Endpoint запроса/api​/v1​/Authors/, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом:{ {  "id": -35,  "idBook": 0,  "firstName": "string",  "lastName": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса,URL, заголовки и тело|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-9edef13e037e5741b9d9fd4c64e12570-0afd9dce4b6e2848-00",   "errors": {     "$.id": [       "'U' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 9."     ]   } }|Провален|{   "id": -35,   "idBook": 0,   "firstName": "string",   "lastName": "string" }| |
| | | |Проверить структуру ответа|В ответе должен присутствовать статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'U' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 9."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|86| POST недопустимое значение: idBook, отрицательные значения| | | | | |Major|
| | |Endpoint запроса/api​/v1​/Authors/, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": 35,  "idBook":-58,  "firstName": "string",  "lastName": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса,URL, заголовки и тело|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-94d7b02e855bac48862d50cb123b5326-0e0385e66393914e-00",   "errors": {     "$.idBook": [       "'%' is an invalid end of a number. Expected a delimiter. Path: $.idBook " LineNumber: 2 " BytePositionInLine: 13."     ]   } }|Провален|{   "id": 35,   "idBook": -58,   "firstName": "string",   "lastName": "string" }| |
| | | |Проверить структуру ответа|В ответе должен присутствовать статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.idBook": [       "'%' is an invalid end of a number. Expected a delimiter. Path: $.idBook " LineNumber: 2 " BytePositionInLine: 13."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|104|GET запрос негативный (id-1)| | | | | |Trivial|
| | |Endpoint запроса| |​​/api​/v1​/Authors​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Authors/-1" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-338cdba65aeafb4f94500ed35c567dd7-03282b20d8bb464c-00",   "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 22."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-67cdb31cbd3f444ca4b7c9ec8f5275a8-dc9842b3debb4549-00" }| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента:   "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 22."     ]   }|Провален|"title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|105|GET запрос негативный (id0)| | | | | |Major|
| | |Endpoint запроса| |​​/api​/v1​/Authors​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Authors/0" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-338cdba65aeafb4f94500ed35c567dd7-03282b20d8bb464c-00",   "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 22."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-67cdb31cbd3f444ca4b7c9ec8f5275a8-dc9842b3debb4549-00" }| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента:   "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 22."     ]   }|Провален|"title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|147| PUT невалидное значение ID в запросе, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/Authors/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=-25 методом PUT с телом: {  "id": 25,  "idBook": 50,  "firstName": "string",  "lastName": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-026c152c5c4c114c9e9742a723fda214-d80fcf13d7415246-00",   "errors": {     "id": [       "The value '-25' is not valid."     ]   } }|Провален|{   "id": 25,   "idBook": 50,   "firstName": "string",   "lastName": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "id": [       "The value '-25' is not valid."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|152|PUT невалидное значение ID в теле запроса, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/Authors/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=25 методом PUT с телом: {  "id": -25,  "idBook": 50,  "firstName": "string",  "lastName": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-7e223d08bef7b44eaf54991fb4500f42-7ff3556297498643-00",   "errors": {     "$.id": [       "'%' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   } }|Провален|{   "id": -25,   "idBook": 50,   "firstName": "string",   "lastName": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'%' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   }|Провален|ошибки не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|196| DELETE запрос негативный (id-1)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Authors/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=-1 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Authors/-1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|В теле ответа должен быть указан статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-65d6c3941f56f24b9308065b4be0423e-d4e833e9999d754a-00",   "errors": {     "id": [       "The value '9999999999' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date:  server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|20| PUT запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |​/api​/v1​/Books​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для изменения активности с id=2 методом PUT с телом:  {  "id": 1,  "title": "TestTitle",  "description": "Test-description",  "pageCount": 1,  "excerpt": "string",  "publishDate": "2023-05-11T21:52:42.363Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X PUT "https://fakerestapi.azurewebsites.net/api/v1/Books/2" -H  "accept: */*" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":1,\"title\":\"TestTitle\",\"description\":\"Test-description\",\"pageCount\":1,\"excerpt\":\"string\",\"publishDate\":\"2023-05-11T21:52:42.363Z\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 1,   "title": "TestTitle",   "description": "Test-description",   "pageCount": 1,   "excerpt": "string",   "publishDate": "2023-05-11T21:52:42.363Z" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:    id - integer($int32) title - string, nullable: true description - string, nullable: true pageCount - integer($int32) excerpt - string nullable: true publishDate - string($date-time)|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date:  server: Kestrel|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=1|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 1,   "title": "TestTitle",   "description": "Test-description",   "pageCount": 1,   "excerpt": "string",   "publishDate": "2023-05-11T21:52:42.363Z" }|Провален|{   "id": 1,   "title": "Book 1",   "description": "Lorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\n",   "pageCount": 100,   "excerpt": "Lorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\n",   "publishDate": "2023-05-21T21:01:24.8144384+00:00" }| |
|22|DELETE запрос позитивный (id2)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Books/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=2 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id=2, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Books/2" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0 content-length: 0  date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=2|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d643f417984b464d8450b50a851e5023-ae5d68c258ad1047-00" }|Провален|{   "id": 2,   "title": "Book 2",   "description": "Lorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\n",   "pageCount": 200,   "excerpt": "Lorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\n",   "publishDate": "2023-05-20T21:00:52.8684432+00:00" }| |
|52|POST запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |​/api​/v1​/Books​/|Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": 3,  "title": "string",  "description": "string",  "pageCount": 0,  "excerpt": "string",  "publishDate": "2023-05-12T11:33:25.499Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X POST "https://fakerestapi.azurewebsites.net/api/v1/Books" -H  "accept: */*" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":3,\"title\":\"string\",\"description\":\"string\",\"pageCount\":0,\"excerpt\":\"string\",\"publishDate\":\"2023-05-12T11:33:25.499Z\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 201 OK|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 3,   "title": "string",   "description": "string",   "pageCount": 0,   "excerpt": "string",   "publishDate": "2023-05-12T11:33:25.499Z" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:    id - integer($int32) title - string, nullable: true description - string, nullable: true pageCount - integer($int32) excerpt - string nullable: true publishDate - string($date-time)|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *   api-supported-versions: 1.0   content-type: application/json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=3|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 3,   "title": "string",   "description": "string",   "pageCount": 0,   "excerpt": "string",   "publishDate": "2023-05-12T11:33:25.499Z" }|Провален|{   "id": 3,   "title": "Book 3",   "description": "Lorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\n",   "pageCount": 300,   "excerpt": "Lorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\nLorem lorem lorem. Lorem lorem lorem. Lorem lorem lorem.\n",   "publishDate": "2023-05-19T20:58:41.5358703+00:00" }| |
|95| POST невалидные значения: ID, отрицательные значения| | | | | |Major|
| | |Endpoint запроса ​/api​/v1​/Books​/, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": -85,  "title": "string",  "description": "string",  "pageCount": 0,  "excerpt": "string",  "publishDate": "2023-05-13T15:45:28.515Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: { {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-2ec77feacf165f43a394a4b228236529-020448f280308944-00",   "errors": {     "$.id": [       "'e' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   } }|Провален|{   "id": -85,   "title": "string",   "description": "string",   "pageCount": 0,   "excerpt": "string",   "publishDate": "2023-05-23T07:52:20.321Z" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'e' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|102| POST недопустимое значение pageCount, отрицательные значения| | | | | |Major|
| | |Endpoint запроса ​/api​/v1​/Books​/, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": 2,  "title": "sfg",  "description": "sfg",  "pageCount": -4,  "excerpt": "dsfg",  "publishDate": "2023-05-25T15:45:28.515Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-9e9e4a9ad38b654d9a873e5f9056486f-b125550fd0706245-00",   "errors": {     "$.pageCount": [       "'fgd,\n  \"excerpt\": \"string\",\n  \"publishDate\": \"2023-05-14T11:54:08.215Z\"\n}' is an invalid JSON literal. Expected the literal 'false'. Path: $.pageCount " LineNumber: 4 " BytePositionInLine: 16."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-261c351719fcc34999fb1b1c989486d9-66656c97163daa4e-00",   "errors": {     "$": [       "'0xC2' is an invalid start of a property name. Expected a '\"'. Path: $ " LineNumber: 1 " BytePositionInLine: 0."     ]   } }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.pageCount": [       "'fgd,\n  \"excerpt\": \"string\",\n  \"publishDate\": \"2023-05-14T11:54:08.215Z\"\n}' is an invalid JSON literal. Expected the literal 'false'. Path: $.pageCount " LineNumber: 4 " BytePositionInLine: 16."     ]   }|Провален|   "$": [       "'0xC2' is an invalid start of a property name. Expected a '\"'. Path: $ " LineNumber: 1 " BytePositionInLine: 0."     ]   } }| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|108|GET запрос негативный (id-1)| | | | | |Trivial|
| | |Endpoint запроса| |​/api​/v1​/Books​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Books/-1" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-bd2fafd19e89e540a29c36028c3464f0-bccdd0bd106a084b-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-5c804cc9056b4f40b4a50d0f4e69c05c-459e855212509444-00" }| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента.   "title": "Bad Request",   "status": 400,|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date:  server: Kestrel|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|109|GET запрос негативный (id0)| | | | | |Major|
| | |Endpoint запроса| |​/api​/v1​/Books​/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Books/0" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type":   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-c423b844b3e9ee4abbb1c8e8f3203490-820c98b2bfbbbd41-00" }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d6e07aa6d32d6b4c9caaacf3ecc5e9e8-4cdd83807311d046-00" }| |
| | | |Проверить структуру ответа|В теле ответа указан статус ответа и причина ошибки на стороне клиента.   "title": "Not Found",   "status": 404,   "traceId": "00-c423b844b3e9ee4abbb1c8e8f3203490-820c98b2bfbbbd41-00" }|Провален| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|158| PUT отрицательное значение ID в запросе| | | | | |Major|
| | |Endpoint запроса /api/v1/Books/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=-12 методом PUT с телом: {  "id": 12,  "title": "string",  "description": "string",  "pageCount": 80,  "excerpt": "string",  "publishDate": "2023-05-15T07:42:50.143Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-52900e6d10f9ca4fa0053ec943a98f17-21bffa3652ad5940-00",   "errors": {     "id": [       "The value '2147483648' is not valid."     ]   } }|Провален|{   "id": 12,   "title": "string",   "description": "string",   "pageCount": 80,   "excerpt": "string",   "publishDate": "2023-05-22T19:42:59.256Z" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "id": [       "The value '2147483648' is not valid."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|164| PUT невалидное значение ID в теле запроса, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/Books/{id}, Headers request: Content-Type: application/json| | | | | |
| | | |Инициировать отправку запроса для изменения сущности с id=12 методом PUT с телом: {  "id": -12,  "title": "string",  "description": "string",  "pageCount": 0,  "excerpt": "string",  "publishDate": "2023-05-15T08:24:19.826Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Успешен|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-010c4cf37d592d448d51afe43b22f7c7-3859b4789c8e484a-00",   "errors": {     "$.id": [       "'*' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 10."     ]   } }|Провален|{   "id": -12,   "title": "string",   "description": "string",   "pageCount": 0,   "excerpt": "string",   "publishDate": "2023-05-22T19:42:59.256Z" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'*' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 10."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|167| PUT невалидные значения: pageCount, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/Books/{id}, Headers request: Content-Type: application/json| | | | | |
| | | |Инициировать отправку запроса для изменения сущности с id=12 методом PUT с телом: {  "id": 12,  "title": "string",  "description": "string",  "pageCount": -6,  "excerpt": "string",  "publishDate": "2023-05-15T08:24:19.826Z" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-0006446dd3744d429af5eb5cdc0f938d-384dad4cc0d95945-00",   "errors": {     "$.pageCount": [       "'g' is an invalid start of a value. Path: $.pageCount " LineNumber: 4 " BytePositionInLine: 15."     ]   } }|Провален|{   "id": 12,   "title": "string",   "description": "string",   "pageCount": -6,   "excerpt": "string",   "publishDate": "2023-05-22T19:42:59.256Z" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.pageCount": [       "'g' is an invalid start of a value. Path: $.pageCount " LineNumber: 4 " BytePositionInLine: 15."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|193| DELETE запрос негативный (id-1)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Books/{id}| | | |
| | | |Инициировать отправку запроса для удаления сущности с id=-1 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Books/-1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|В теле ответа должен быть указан статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-3db6ef5373be604c8175598edb68c064-b73c5d748c4d9747-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/problem+json; charset=utf-8 date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|194|DELETE запрос позитивный (id200)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Books/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления сущности с id=200 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id=200, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Books/200" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0 content-length: 0  date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=2|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d643f417984b464d8450b50a851e5023-ae5d68c258ad1047-00" }|Провален|тело ответа отсутствует| |
|195| DELETE запрос негативный (id201)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Books/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=201 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Books/201" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|В теле ответа должен быть указан статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "Not Found.",   "status": 404,   "traceId": "00-3db6ef5373be604c8175598edb68c064-b73c5d748c4d9747-00",   "errors": {     "id": [       "The value '201' is not found."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/problem+json; charset=utf-8 date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|24| PUT запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=1 методом PUT с телом:  {  "id": 5,  "idBook": 7,  "url": "Teststring" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X PUT "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/1" -H  "accept: text/plain; v=1.0" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":5,\"idBook\":7,\"url\":\"Teststring\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 5,   "idBook": 7,   "url": "Teststring" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:    id - integer($int32) idBook - integer($int32) url - string, nullable: true|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date:  server: Kestrel|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=5|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 5,   "idBook": 7,   "url": "Teststring" }|Провален|{   "id": 5,   "idBook": 5,   "url": "https://placeholdit.imgix.net/~text?txtsize=33&txt=Book 5&w=250&h=350" }| |
|26|DELETE запрос позитивный (id1)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления сущности с id=1 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0 content-length: 0  date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=1|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d643f417984b464d8450b50a851e5023-ae5d68c258ad1047-00" }|Провален|{   "id": 1,   "idBook": 1,   "url": "https://placeholdit.imgix.net/~text?txtsize=33&txt=Book 1&w=250&h=350" }| |
|54|POST запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/CoverPhotos|Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": 1,  "idBook": 1,  "url": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X POST "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos" -H  "accept: text/plain; v=1.0" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":1,\"idBook\":1,\"url\":\"string\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 1,   "idBook": 1,   "url": "string" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:    id - integer($int32) idBook - integer($int32) url - string($uri), nullable: true|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  api-supported-versions: 1.0  content-type: application/json; charset=utf-8; v=1.0  date: Fri12 May 2023 15:34:35 GMT  server: Kestrel  transfer-encoding: chunked|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=1|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 201 OK|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 1,   "idBook": 1,   "url": "string" }|Провален|{   "id": 1,   "idBook": 1,   "url": "https://placeholdit.imgix.net/~text?txtsize=33&txt=Book 1&w=250&h=350" }| |
|112| GET{idBook} запрос негативный (idBook-1)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/CoverPhotos/books/covers/{idBook}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с idBook=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/books/covers/-1" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-f965b92af1e35049b56cd27d0c803b0a-524ac50f9e07d047-00",   "errors": {     "idBook": [       "The value '-1' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить структуру ответа|В структуре ответа имеется статус ответа и указана причина, вызвавшая ошибку   "errors": {     "idBook": [       "The value '-1' is not valid."     ]   } }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|113| GET{idBook} запрос негативный (idBook0)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/CoverPhotos/books/covers/{idBook}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с idBook=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/books/covers/0" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-87001773c3db544f8af3fd2011ef7a1b-af8bdf4a93fe1345-00" }|Провален|тело ответа отсутствует| |
| | | |Проверить структуру ответа|В структуре ответа имеется статус ответа и указана причина, вызвавшая ошибку   "title": "Not Found",   "status": 404,   "traceId": "00-87001773c3db544f8af3fd2011ef7a1b-af8bdf4a93fe1345-00" }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|115| GET{idBook} запрос негативный (idBook201)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/CoverPhotos/books/covers/{idBook}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с idBook=201 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/books/covers/201" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-081b407e6097fe48b17e9cd5fc4b163a-be2f6cbd71dc9a49-00" }|Провален|тело ответа отсутствует| |
| | | |Проверить структуру ответа|В структуре ответа имеется статус ответа и указана причина, вызвавшая ошибку   "title": "Not Found",   "status": 404,   "traceId": "00-081b407e6097fe48b17e9cd5fc4b163a-be2f6cbd71dc9a49-00" }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|116|GET запрос негативный (id-1)| | | | | |Trivial|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/-1" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-e641bf0a4d18804489f6387022d390e4-864afc53f8907842-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-821f4da259d082468a2d5957df4aa6a4-7c4f58d666916740-00" }| |
| | | |Проверить структуру ответа|В структуре ответа имеется статус ответа и указана причина, вызвавшая ошибку   "title": "Bad Request",   "status": 400,|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|117|GET запрос негативный (id0)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/0" -H  "accept: text/plain; v=1.0"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-e641bf0a4d18804489f6387022d390e4-864afc53f8907842-00",   "errors": {     "id": [       "The value '0' is not valid."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-63c0a05f722231419178c924fa7bcc14-1c1b126343d0134a-00" }| |
| | | |Проверить структуру ответа|В структуре ответа имеется статус ответа и указана причина, вызвавшая ошибку   "title": "Bad Request",   "status": 400,|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|132| POST невалидные значения: ID, idBook, отрицательные значения| | | | | |Major|
| | |Endpoint запроса/api​/v1​//api/v1/CoverPhotos/, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": -1,  "idBook": -1,  "url": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса,URL, заголовки и тело|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-122144446a3b0849b4c920f49f768e0b-d732fa53b63c924d-00",   "errors": {     "$.id": [       "'$' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   } }|Провален|{   "id": -1,   "idBook": -1,   "url": "string" }| |
| | | |Проверить структуру ответа|В ответе должен присутствовать статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'$' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|172| PUT невалидное значение ID в запросе, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/CoverPhotos/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=-2 методом PUT с телом: {  "id": 2,  "idBook": 2,  "url": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-d46a9f19dec3dc498d65f79549a302fd-9eb3214a58077a47-00",   "errors": {     "id": [       "The value '-2' is not valid."     ]   } }|Провален|{   "id": 2,   "idBook": 2,   "url": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "id": [       "The value '-2' is not valid."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|179| PUT не валидное значение ID в теле запроса, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/CoverPhotos/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=2 методом PUT с телом: {  "id": -2,  "idBook": 2,  "url": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-5e315513cac555408b3b5e486edc474f-d4e222d9a61f6849-00",   "errors": {     "$.id": [       "'m' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 7."     ]   } }|Провален|{   "id": -2,   "idBook": 2,   "url": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'m' is an invalid start of a value. Path: $.id " LineNumber: 1 "BytePositionInLine: 7."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|180| PUT невалидное значение idBook, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/CoverPhotos/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=2 методом PUT с телом: {  "id": 2,  "idBook": -2,  "url": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-0b917eb58a3f09429ffce07517e18975-a9b91bfd0bdbf340-00",   "errors": {     "$.idBook": [       "'!' is an invalid end of a number. Expected a delimiter. Path: $.idBook " LineNumber: 2 " BytePositionInLine: 12."     ]   } }|Провален|{   "id": -2,   "idBook": 0,   "url": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.idBook": [       "'!' is an invalid end of a number. Expected a delimiter. Path: $.idBook " LineNumber: 2 " BytePositionInLine: 12."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|190| DELETE запрос негативный (id-1)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=-1 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/-1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа|Тело должно содержать статус и причину ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-16c0fa821b0ec048874de075fc6cfef9-1897e23f9fb01940-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-0e7aeacf80f76648acf0e2770733952c-255ba38fa313b84d-00" }| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/problem+json; charset=utf-8 date: server: Kestre|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|191| DELETE запрос позитивный (id200)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=200 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/200" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0 content-length: 0  date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=200|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d643f417984b464d8450b50a851e5023-ae5d68c258ad1047-00" }|Провален|{   "id": 200,   "idBook": 200,   "url": "https://placeholdit.imgix.net/~text?txtsize=33&txt=Book 200&w=250&h=350" }| |
|192| DELETE запрос негативный (id201)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/CoverPhotos/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=201 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/CoverPhotos/201" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа|Тело должно содержать статус и причину ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-d643f417984b464d8450b50a851e5023-ae5d68c258ad1047-00" }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/problem+json; charset=utf-8 date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|28| PUT запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для изменения активности с id=3 методом PUT с телом:  {  "id": 5,  "userName": "Teststring",  "password": "Test" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X PUT "https://fakerestapi.azurewebsites.net/api/v1/Users/3" -H  "accept: */*" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":5,\"userName\":\"Teststring\",\"password\":\"Test\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 5,   "userName": "Teststring",   "password": "Test" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:    id - integer($int32) userName - string, nullable: true password - string, nullable: true|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/json; charset=utf-8 date: server: Kestrel  transfer-encoding: chunked|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=5|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 5,   "userName": "Teststring",   "password": "Test" }|Провален|{   "id": 5,   "userName": "User 5",   "password": "Password5" }| |
|30|DELETE запрос позитивный (id3)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=3 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Users/3" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0 content-length: 0  date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=3|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет примерный внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-da7c8e8cef2878409d718bd83cf372a8-8bd358e19fa1024b-00" }|Провален|{   "id": 3,   "userName": "User 3",   "password": "Password3" }| |
|56|POST запрос позитивный| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Users|Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": 1,  "userName": "string",  "password": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X POST "https://fakerestapi.azurewebsites.net/api/v1/Users" -H  "accept: */*" -H  "Content-Type: application/json; v=1.0" -d "{\"id\":1,\"userName\":\"string\",\"password\":\"string\"}"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 201 OK|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "id": 1,   "userName": "string",   "password": "string" }|Успешен| | |
| | | |Проверить структуру ответа|Имена и типы полей соответствуют ожидаемым  Значения полей соответствуют ожидаемым значениям из тестовых данных:    id - integer($int32) userName - string, nullable: true password - string, nullable: true|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  api-supported-versions: 1.0  content-type: application/json; charset=utf-8; v=1.0  date:  server: Kestrel  transfer-encoding: chunked|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов измененной сущности методом GET по ее ID=1|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:  {   "id": 1,   "userName": "string",   "password": "string" }|Провален|{   "id": 1,   "userName": "User 1",   "password": "Password1" }| |
|120|GET запрос негативный (id0)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=0 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Users/0" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-6145002b15f6464ab8d8ca2226f278ff-f21cd7c2d3823042-00",   "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 17."     ]   } }|Провален|   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-bc736af87df3f148acc3078c86b2bd6f-63ed4f64b59ab847-00" }| |
| | | |Проверить структуру ответа|В структуре ответа должен быть статус и причина возникновения ошибки  "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 17."     ]   }|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|121|GET запрос негативный (id-1)| | | | | |Trivial|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для получения информации с сервера с id=-1 методом GET|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X GET "https://fakerestapi.azurewebsites.net/api/v1/Users/-1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|404 Error: Not Found| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-6145002b15f6464ab8d8ca2226f278ff-f21cd7c2d3823042-00",   "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 17."     ]   } }|Провален|{   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-47515e055da4e3479cdf8cef3c50e4ba-ed1bc78d84293f4c-00" }| |
| | | |Проверить структуру ответа|В структуре ответа должен быть статус и причина возникновения ошибки    "errors": {     "$.id": [       "The JSON value could not be converted to System.Int32. Path: $.id " LineNumber: 0 " BytePositionInLine: 17."     ]   }|Провален|  "title": "Not Found",   "status": 404,| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0   content-type: application/problem+json; charset=utf-8; v=1.0|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|137| POST невалидное значение ID: отрицательное значение| | | | | |Major|
| | |Endpoint запроса/api​/v1​//api/v1/Users/, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса методом POST для добавления сущности с телом: {  "id": -2,  "userName": "string",  "password": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса,URL, заголовки и тело|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-b24bced560420244a1a9091a46a98e04-7feb7ad9d337c342-00",   "errors": {     "$.id": [       "'a' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   } }|Провален|  {   "id": -2,   "userName": "string",   "password": "string" }| |
| | | |Проверить структуру ответа|В ответе должен присутствовать статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'a' is an invalid start of a value. Path: $.id " LineNumber: 1 " BytePositionInLine: 8."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|182| PUT отрицательное значение ID в запросе| | | | | |Major|
| | |Endpoint запроса /api/v1/Users/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=-16 методом PUT с телом: {  "id": 16,  "userName": "string",  "password": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид: {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-86849e672f6bea44ba29d1e04d935fcc-04b9e6c281ce1747-00",   "errors": {     "id": [       "The value '-16' is not valid."     ]   } }|Провален|{   "id": 16,   "userName": "string",   "password": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "id": [       "The value '-16' is not valid."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|185| PUT невалидное значение ID в теле запроса, отрицательное значение| | | | | |Major|
| | |Endpoint запроса /api/v1/Users/{id}, Headers request: Content-Type: application/json| | |Успешен| | |
| | | |Инициировать отправку запроса для изменения сущности с id=16 методом PUT с телом: {  "id": -16,  "userName": "string",  "password": "string" }|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL|CURL содержит метод запроса, URL, заголовки и тело запроса|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет внешний вид:   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-a9daa4ead41a8e4985f0562c2edcb44f-09e0ad83f7515b41-00",   "errors": {     "$.id": [       "'#' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 10."     ]   } }|Провален|{   "id": 0,   "userName": "string",   "password": "string" }| |
| | | |Проверить структуру ответа|В ответе присутствует статус ответа и причина возникновения ошибки на стороне клиента:   "errors": {     "$.id": [       "'#' is an invalid end of a number. Expected a delimiter. Path: $.id " LineNumber: 1 " BytePositionInLine: 10."     ]   }|Провален|ошибка не возникает | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   access-control-allow-origin: *  content-type: application/problem+json; charset=utf-8|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|187| DELETE запрос негативный (id-1)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=-1 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Users/-1" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело запроса|В структуре тела имеется статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-f6cdb248706c8c45869383b77c33fced-07c43f49dacd6143-00",   "errors": {     "id": [       "The value '-1' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/problem+json; charset=utf-8  date:  server: Kestrel transfer-encoding: chunked|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
|188|DELETE запрос позитивный (id10)| | | | | |Critical|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=10 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Users/10" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 200 OK|Успешен| | |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"   api-supported-versions: 1.0 content-length: 0  date: server: Kestre|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
| | | |Инициировать вызов удаленной активности методом GET по ее ID=10|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить код состояния|HTTP статус 404 Error: Not Found|Провален|HTTP статус 200 OK| |
| | | |Проверить тело ответа от сервера|Тело ответа в формате JSON возвращается от сервера и имеет примерный внешний вид:  {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",   "title": "Not Found",   "status": 404,   "traceId": "00-da7c8e8cef2878409d718bd83cf372a8-8bd358e19fa1024b-00" }|Провален|{   "id": 10,   "userName": "User 10",   "password": "Password10" }| |
|189| DELETE запрос негативный (id11)| | | | | |Major|
| | |Endpoint запроса| |/api/v1/Users/{id}|Успешен| | |
| | | |Инициировать отправку запроса для удаления активности с id=11 методом DELETE|Запрос успешно отправлен на сервер|Успешен| | |
| | | |Проверить правильность формирования CURL (запрос, URL с указанным id, тело запроса)|curl -X DELETE "https://fakerestapi.azurewebsites.net/api/v1/Users/11" -H  "accept: */*"|Успешен| | |
| | | |Проверить код состояния|HTTP статус 400 Error: Bad Request|Провален|HTTP статус 200 OK| |
| | | |Проверить тело запроса|В структуре тела имеется статус и причина возникновения ошибки {   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",   "title": "One or more validation errors occurred.",   "status": 400,   "traceId": "00-f6cdb248706c8c45869383b77c33fced-07c43f49dacd6143-00",   "errors": {     "id": [       "The value '11' is not valid."     ]   } }|Провален|тело ответа отсутствует| |
| | | |Проверить заголовки ответа|Соответствует формату HTTP "Имя: Значение"  content-type: application/problem+json; charset=utf-8  date:  server: Kestrel transfer-encoding: chunked|Успешен| | |
| | | |Проверить время выполнения запроса|Скорость отклика на запрос приемлемая|Успешен| | |
