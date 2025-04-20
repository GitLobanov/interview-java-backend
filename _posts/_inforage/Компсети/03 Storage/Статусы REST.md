1xx: **Информационные**
100 Continue – Сервер получил заголовки запроса, клиент может продолжать отправку тела.
101 Switching Protocols – Сервер согласен сменить протокол (например, на WebSocket).
102 Processing – Запрос принят, но обработка ещё не завершена (используется в WebDAV).
103 Early Hints – Предварительные подсказки (например, для предзагрузки ресурсов).

2xx: **Успешные**
200 OK – Стандартный успешный ответ (есть тело ответа).
201 Created – Ресурс успешно создан (обычно в POST с Location в заголовках).
202 Accepted – Запрос принят, но обработка ещё не завершена (асинхронные задачи).
203 Non-Authoritative Information – Ответ успешен, но метаданные могут отличаться.
204 No Content – Успешно, но без тела ответа (например, после DELETE).
205 Reset Content – Успешно, клиент должен сбросить представление документа.
206 Partial Content – Частичный ответ (например, при загрузке файла по диапазонам).
207 Multi-Status – Множественный статус (WebDAV).
208 Already Reported – Ресурс уже перечислен (WebDAV).
226 IM Used – Сервер выполнил GET с учетом If-Modified-Since.

3xx: **Перенаправления**
300 Multiple Choices – Несколько вариантов ответа (редко используется).
301 Moved Permanently – Ресурс перемещён навсегда (новый URL в Location).
302 Found – Временное перенаправление (старый код, лучше использовать 307/308).
303 See Other – Ответ доступен по другому URL (обычно после POST).
304 Not Modified – Ресурс не изменился (кеширование, If-Modified-Since).
305 Use Proxy – Устаревший (использовать прокси из Location).
307 Temporary Redirect – Временное перенаправление с сохранением метода.
308 Permanent Redirect – Постоянное перенаправление с сохранением метода.

4xx: Ошибки клиента
400 Bad Request – Неверный синтаксис запроса (валидация, некорректный JSON).
401 Unauthorized – Требуется аутентификация (например, неверный токен).
402 Payment Required – Зарезервирован для будущего использования.
403 Forbidden – Доступ запрещён (нет прав).
404 Not Found – Ресурс не найден.
405 Method Not Allowed – Метод не поддерживается (например, PUT на read-only endpoint).
406 Not Acceptable – Сервер не может вернуть ответ в нужном формате (Accept-заголовки).
407 Proxy Authentication Required – Требуется аутентификация для прокси.
408 Request Timeout – Сервер не дождался запроса.
409 Conflict – Конфликт (например, попытка создать дубликат).
410 Gone – Ресурс удалён навсегда (в отличие от 404, здесь сервер знал о ресурсе).
411 Length Required – Требуется заголовок Content-Length.
412 Precondition Failed – Условие в If-Match не выполнено.
413 Payload Too Large – Тело запроса превышает лимит.
414 URI Too Long – URL слишком длинный.
415 Unsupported Media Type – Неподдерживаемый формат данных (Content-Type).
416 Range Not Satisfiable – Неверный диапазон для Range-заголовка.
417 Expectation Failed – Сервер не может выполнить условие из Expect.
418 I'm a Teapot – Шуточный статус (RFC 2324).
422 Unprocessable Entity – Данные верны, но сервер не может их обработать (например, семантическая ошибка).
423 Locked – Ресурс заблокирован (WebDAV).
424 Failed Dependency – Зависимый запрос провалился (WebDAV).
425 Too Early – Запрос отправлен слишком рано (риск повторной атаки).
426 Upgrade Required – Требуется обновление протокола.
428 Precondition Required – Требуется условный запрос (If-Match).
429 Too Many Requests – Превышен лимит запросов (rate limiting).
431 Request Header Fields Too Large – Заголовки слишком большие.
451 Unavailable For Legal Reasons – Доступ запрещён по юридическим причинам.

5xx: Ошибки сервера
500 Internal Server Error – Общая ошибка сервера (необработанное исключение).
501 Not Implemented – Метод не реализован.
502 Bad Gateway – Прокси/шлюз получил неверный ответ от upstream-сервера.
503 Service Unavailable – Сервис временно недоступен (перегрузка, техработы).
504 Gateway Timeout – Шлюз не дождался ответа от upstream-сервера.
505 HTTP Version Not Supported – Версия HTTP не поддерживается.
506 Variant Also Negotiates – Ошибка контент-negotiation.
507 Insufficient Storage – Недостаточно места (WebDAV).
508 Loop Detected – Обнаружена бесконечная переадресация (WebDAV).
510 Not Extended – Требуется расширение запроса.
511 Network Authentication Required – Требуется аутентификация в сети (например, в публичном Wi-Fi).



 **Самые важные статусы для REST API**

| Статус                    | Когда использовать?         |
| ------------------------- | --------------------------- |
| 200 OK                    | Успешный GET/PUT/PATCH.     |
| 201 Created               | После успешного POST.       |
| 204 No Content            | После успешного DELETE.     |
| 400 Bad Request           | Ошибка валидации.           |
| 401 Unauthorized          | Требуется аутентификация.   |
| 403 Forbidden             | Нет прав.                   |
| 404 Not Found             | Ресурс не существует.       |
| 429 Too Many Requests     | Rate limiting.              |
| 500 Internal Server Error | Непойманная ошибка сервера. |