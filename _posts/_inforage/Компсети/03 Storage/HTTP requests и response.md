
Client send request:

```http
GET /about.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

Запросы состоят из следующих элементов:

1. **HTTP-метод**. Обычно глагол типа _GET_, _POST_ и так далее. [Список методов](https://developer.mozilla.org/ru/docs/Web/HTTP/Methods).
2. **Путь ресурса**. Указывается путь к сайту. Например в URL `https://jusan.kz/jusan-singularity` путем является `/jusan-singularity`.
3. **Версия протокола HTTP**.
4. **HTTP-заголовки**. Являются необязательными. Обычно передают дополнительную информацию для серверов.
5. **Тело**. Передается для некоторых методов, таких как POST, которые содержат полезную информацию. К примеру JSON.

Server send a response:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 2048

<html>
  <head><title>About Us</title></head>
  <body><h1>About Us</h1><p>Welcome to our company!</p></body>
</html>
```

Ответы состоят из следующих элементов:

1. **Версия протокола HTTP**, которому они следуют.
2. **Код статуса**, указывающий, был ли запрос успешным или нет, и почему.
3. **Текст статуса**, краткое описание кода статуса.
4. **HTTP-заголовки**
5. **Тело**