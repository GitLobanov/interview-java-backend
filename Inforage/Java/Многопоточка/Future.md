Когда мы используем `ExecutorService`, важно понимать значение такого объекта как `Future`.

```coffeescript
Future<Response> future = executorService.submit(() -> sendRequestToRemoteServer());
```

`Future` олицетворяет контейнер, значение в котором либо уже присутствует, либо появится со временем. Чтобы получить результат, достаточно вызвать метод `get`.

```vbscript
Future<Response> future = executorService.submit(() -> sendRequestToRemoteServer());
...
Response response = future.get();
```

Однако проблема в том, что метод `get` является _блокирующим_. Это значит, что поток будет в ожидании результата до тех пор, пока он не появится. Поэтому есть вариация `get` с timeout.

```vbscript
Future<Response> future = executorService.submit(() -> sendRequestToRemoteServer());
...
Response response = future.get(3, TimeUnit.SECONDS);
```

В данном случае результат будет ожидаться не более трех секунд. В противном выброситься `TimeoutException`.