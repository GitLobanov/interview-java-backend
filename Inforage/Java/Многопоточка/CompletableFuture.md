---
Определение: Особая реализация интерфейса Future, которая добавляет новые возможности, такие как возможность вручную завершать задачи, связывать несколько задач друг с другом, а также цепочку последовательных асинхронных вызовов.
Примечание: Класс в Java (начиная с версии 8), который позволяет работать с асинхронными операциями.
---
## Методы

- **Создание задач:**
    - `CompletableFuture.supplyAsync()` — запускает асинхронную задачу, возвращающую результат.
    - `CompletableFuture.runAsync()` — запускает асинхронную задачу без возвращаемого результата.
- **Композиция и объединение:**
    - `thenApply()` — выполняется после завершения предыдущей задачи и обрабатывает результат.
    - `thenAccept()` — принимает результат предыдущей задачи, но не возвращает значения.
    - `thenRun()` — выполняет код после завершения предыдущей задачи, но не использует результат.
    - `thenCompose()` — комбинирует две асинхронные задачи последовательно.
    - `thenCombine()` — объединяет две задачи и работает с их результатами.
- **Завершение задач:**
    - `complete()` — вручную завершает задачу с результатом.
    - `completeExceptionally()` — завершает задачу с ошибкой.
- **Работа с ошибками:**
    - `exceptionally()` — обрабатывает исключения, если они произошли в процессе выполнения задачи.
    - `handle()` — обрабатывает как результат задачи, так и возможные исключения.
- **Методы ожидания:**
    - `get()` — блокирует выполнение, пока задача не завершится и не вернет результат.
    - `join()` — аналогичен `get()`, но без необходимости обработки исключений.

## Примеры

**Асинхронная задача с результатом:**

```java
    public CompletableFuture<User> getUserInfoAsync(int userId) {
        return CompletableFuture.supplyAsync(() -> {
            // Имитируем задержку вызова внешнего API
            simulateDelay(1000);
            System.out.println("Получена информация о пользователе");
            return new User(userId, "John Doe", "john.doe@email.com");
        });
    }

    // Асинхронное получение списка заказов пользователя
    public CompletableFuture<Order[]> getUserOrdersAsync(int userId) {
        return CompletableFuture.supplyAsync(() -> {
            // Имитируем задержку вызова внешнего API
            simulateDelay(1200);
            System.out.println("Получен список заказов пользователя");
            return new Order[]{
                    new Order(1, "Laptop", 1200),
                    new Order(2, "Smartphone", 800)
            };
        });
    }

    // Асинхронное получение данных о балансе пользователя
    public CompletableFuture<Balance> getUserBalanceAsync(int userId) {
        return CompletableFuture.supplyAsync(() -> {
            // Имитируем задержку вызова внешнего API
            simulateDelay(800);
            System.out.println("Получен баланс пользователя");
            return new Balance(userId, 1500);
        });
    }

    // Основной метод для сбора всех данных
    public CompletableFuture<UserProfile> getUserProfileAsync(int userId) {
        CompletableFuture<User> userInfoFuture = getUserInfoAsync(userId);
        CompletableFuture<Order[]> userOrdersFuture = getUserOrdersAsync(userId);
        CompletableFuture<Balance> userBalanceFuture = getUserBalanceAsync(userId);

        // Собираем все результаты в один объект
        return CompletableFuture.allOf(userInfoFuture, userOrdersFuture, userBalanceFuture)
                .thenApply(voidResult -> {
                    try {
                        User user = userInfoFuture.get();
                        Order[] orders = userOrdersFuture.get();
                        Balance balance = userBalanceFuture.get();

                        // Создаем объект UserProfile с собранными данными
                        return new UserProfile(user, orders, balance);
                    } catch (InterruptedException | ExecutionException e) {
                        throw new RuntimeException(e);
                    }
                });
    }
```

**Обработка исключений:**

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    if (true) {
        throw new RuntimeException("Ошибка!");
    }
    return "Не выполнится";
}).exceptionally(ex -> "Возникла ошибка: " + ex.getMessage());

System.out.println(future.get()); // Вывод: Возникла ошибка: Ошибка!
```

### Создание задачи

#### supplyAsync

Если нужно выполнить асинхронно задачу и вернуть результат - есть метод `supplyAsync`:

```coffeescript
    CompletableFuture<String> completableFuture = CompletableFuture
            .supplyAsync(() -> GroundControl.getStatus());
```

#### runAsync

Если нам нужно выполнить какую-то работу без возврата результата выполнения, можно воспользоваться `runAsync`:

```coffeescript
    CompletableFuture<Void> completableFuture = CompletableFuture
            .runAsync(() -> MajorTom.sendStatus());
```

### Executor

Мы уже знаем, что `supplyAsync` и `runAsync` запускают задачи в новом потоке. Но откуда берется новый поток?

Ответ: он получается из общего `ForkJoinPool.commonPool()`. Но мы можем использовать и свой пул:

```coffeescript
    Executor executor = Executors.newCachedThreadPool();

    CompletableFuture<Void> completableFuture = CompletableFuture
            .runAsync(() -> MajorTom.sendStatus(), executor);
```

### Завершение задачи

Одна из центральных возможностей, которая даже фигурирует в названии - `CompletableFuture`.

```dart
    CompletableFuture<String> completableFuture = new CompletableFuture<>();

    try {
        //заблокирует главный поток на 100 миллисекунд
        completableFuture.get(100, TimeUnit.MILLISECONDS);
    } catch (Exception e){
        logger.warn("Completing future with default value", e);
    } finally {
        completableFuture.complete("Устал ждать");
    }
```

Ура, мы смогли завершить фьючу руками и вернуть результат "Устал ждать".

### Обработка результата

У `CompletableFuture` метод `get()` тоже блокирующий, как и у обычной фьючи. Но зато мы можем навесить на `CompletableFuture` колбек, который вызовется, когда вернется результат задачи.

#### thenApply

```coffeescript
    CompletableFuture<String> createBalanceInfo = CompletableFuture.supplyAsync(() -> {
        long amountOfMoney = Counter.countMoney();
        return amountOfMoney;
    }).thenApply(amountOfMoney -> {
        return "There is a lot of money:" + amountOfMoney;
    });

    logger.info(createBalanceInfo.get());
```

`thenApply` возвращает `CompletableFuture<T>`, поэтому можно сделать цепочку из `thenApply`, которые последовательно будут преобразовывать результат.

#### thenAccept

`thenAccept` в отличие от `thenApply` не возвращает результата выполнения, он просто делает некоторую полезную работу:

```coffeescript
    CompletableFuture<Void> createBalanceInfo = CompletableFuture.supplyAsync(() -> {
        long amountOfMoney = Counter.countMoney();
        return amountOfMoney;
    }).thenAccept(amountOfMoney -> send(amountOfMoney));
```

#### thenRun

То же, что и `thenAccept`, только не имеет доступа к результату задачи.

```coffeescript
    CompletableFuture<Void> createBalanceInfo = CompletableFuture.supplyAsync(() -> {
        long amountOfMoney = Counter.countMoney();
        return amountOfMoney;
    }).thenRun(() -> logger.info("Created balance info"));
```

#### async методы

`thenApply`, `thenAccept`, `thenRun` выполняются в том же потоке, в котором и сама `CompletableFuture`, но если мы хотим запустить их в отдельных потоках - мы можем воспользоваться async методами: `CompletableFuture`, предоставляет целых две реализации асинхронных методов для всех колбеков.

```java
    public <U> CompletableFuture<U> thenApplyAsync(              
        Function<? super T,? extends U> fn) {
        return uniApplyStage(defaultExecutor(), fn);
    }

    public <U> CompletableFuture<U> thenApplyAsync(
        Function<? super T,? extends U> fn, Executor executor) {
        return uniApplyStage(screenExecutor(executor), fn);
    }
```

Перепишем пример сверху на async:

```coffeescript
    CompletableFuture<String> createBalanceInfo = CompletableFuture.supplyAsync(() -> {
        long amountOfMoney = Counter.countMoney();
        return amountOfMoney;
    }).thenApplyAsync(amountOfMoney -> {
        return "There is a lot of money:" + amountOfMoney;
    });

    logger.info(createBalanceInfo.get());
```

Теперь колбек будет выполняться в новом потоке, полученным из пула `ForkJoinPool.commonPool()`. Если мы хотим получить поток из своего пула, это можно сделать, указав его вторым аргументом в `thenXXXAsync`:

```coffeescript
    Executor executor = Executors.newFixedThreadPool(2);

    CompletableFuture<String> createBalanceInfo = CompletableFuture.supplyAsync(() -> {
        long amountOfMoney = Counter.countMoney();
        return amountOfMoney;
    }).thenApplyAsync(amountOfMoney -> {
        return "There is a lot of money:" + amountOfMoney;
    }, executor);
```

### Комбинирование результатов

#### Комбинирование двух задач

Допустим у нас есть две зависимые задачи:

```coffeescript
    CompletableFuture<ClientInfo> getClientInfo(String clientId) {
        return CompletableFuture.supplyAsync(() -> ClientService.getClientInfo(clientId));
    }

    CompletableFuture<BalanceInfo> getBalanceInfo (ClientInfo clientInfo){
        return CompletableFuture.supplyAsync(() -> {
            int balanceId = clientInfo.getBalanceId();
            return BalanceService.getBalanceInfo(balanceId);
        });
    }
```

Мы хотим, чтобы вторая запускалась с результатами первой, можно использовать метод `thenCompose()`:

```php-template
    CompletableFuture<BalanceInfo> balanceInfo = getClientInfo(clientId)
            .thenCompose(clientInfo -> getBalanceInfo(clientInfo));
```

Если же у нас есть две независимые задачи, и нам нужно обработать их результат, то можно вызвать метод `thenCombine()`:

```coffeescript
    CompletableFuture<String> completableFuture = CompletableFuture.supplyAsync(() -> "Ground Control to ")
            .thenCombine(CompletableFuture.supplyAsync(() -> "Major Tom"), String::concat);
```

#### Комбинирование нескольких задач

Теперь нам надо запустить несколько задач параллельно и подождать пока они все завершатся: будем использовать `allOf()`. Этот метод возвращает `CompletableFuture<Void>`, чтобы получить результат, мы можем или вызвать `get()` или воспользоваться `CompletableFuture.join()`:

```coffeescript
    CompletableFuture<String> future1 = CompletableFuture
            .supplyAsync(() -> "Ground Control to");
    CompletableFuture<String> future2 = CompletableFuture
            .supplyAsync(() -> "Major Tom");
    CompletableFuture<String> future3 = CompletableFuture
            .supplyAsync(() -> "Take your protein pills and put your helmet on");

    CompletableFuture<Void> completableFuture = CompletableFuture.allOf(future1, future2, future3);

    String combined = Stream.of(future1, future2, future3)
        .map(CompletableFuture::join)
        .collect(Collectors.joining(" "));   // Ground Control to Major Tom Take your protein pill and put your helmet on
```

Если нужен результат только первой закончившей фьючи, то подходит метод `anyOf()`. Единственное, этот метод всегда возвращает `CompletableFuture<Object>`:

```coffeescript
        CompletableFuture<String> future1 = CompletableFuture
                .supplyAsync(() -> "Boris Volynov");
        CompletableFuture<String> future2 = CompletableFuture
                .supplyAsync(() -> "Major Tom");
        CompletableFuture<String> future3 = CompletableFuture
                .supplyAsync(() -> "Valentina Tereshkova");

        CompletableFuture<Object> completableFuture = CompletableFuture.anyOf(future1, future2, future3);
        
        completableFuture.get();
```

### Обработка ошибок и тайм-ауты в CompletableFuture

Метод `exceptionally()` позволяет обработать исключение и вернуть значение, которое заменит результат завершившегося с ошибкой `CompletableFuture`. Метод вызывается только в случае исключения и не активируется при успешном завершении задачи:

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    if (new Random().nextBoolean()) {
        throw new RuntimeException("Что-то пошло не так");
    }
    return "Успех!";
}).exceptionally(ex -> {
    System.out.println("Обработка исключения: " + ex.getMessage());
    return "Восстановлено после ошибки";
});

future.thenAccept(result -> System.out.println("Результат: " + result));
```

Метод `handle()` позволяет обработать как успешный результат, так и исключение, используя `BiFunction`, принимающий результат или исключение в качестве аргументов. Он всегда выполняется независимо от того, была ли ошибка:

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    if (new Random().nextBoolean()) {
        throw new RuntimeException("Что-то пошло не так");
    }
    return "Успех!";
}).handle((result, ex) -> {
    if (ex != null) {
        System.out.println("Обработка исключения: " + ex.getMessage());
        return "Восстановлено после ошибки";
    }
    return result;
});

future.thenAccept(result -> System.out.println("Результат: " + result));
```


## Resources

- [Кратко про класс CompletableFuture в Java](https://habr.com/ru/companies/otus/articles/818955/)
- 