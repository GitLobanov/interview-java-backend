## QA

#### Что такое потокобезопасный класс?

- Во-первых это может быть [[Immutable Class]]
- Для mutable классов можно использовать `synchronized` или более гибкие механизмы, такие как `ReadWriteLock`.
- Через гет нужно возвращать копию объекта, к примеру CopyOnWriteArrayList
- Если что-то передаем через конструктор, нужно копировать полученный объект, и только потом внедрять в поле
- Можно использовать Record или @Value от ломбока, сделает класс иммутабельным

#### Как работает пул потоков?

- [[ExecutorService]]
- [Executors](Executors.md)

Он создает фиксированное количество потоков, которые могут быть повторно использованы для выполнения различных задач, что помогает снизить затраты на создание и уничтожение потоков.

- newSingleThreadPool() - создает пулл с 1 потоком
- newFixedThreadPool(int number) - создает пулл с указанным количеством потоков  
- newCachedThreadPool() - создает динамический пулл потоков
- newScheduledThreadPool() - создает фиксированный пулл потоков, который может выполнять задачки по расписанию  
- newWorkStealingPool() - создает ForkJoinPool

#### Как реализуется синхронизация?

Синхронизация в Java может быть реализована несколькими способами:

1. Синхронизированные методы: Используйте ключевое слово `synchronized` для объявления методов, которые должны быть выполнены одним потоком в данный момент времени.
2. Синхронизированные блоки: Позволяет синхронизировать только определенные блоки кода внутри метода.
3. Объекты блокировки: Используйте `ReentrantLock` для более гибкой синхронизации, включая возможность принудительного ожидания и тайм-аутов.

#### Что такое монитор? Как он выбирается? 

- Монитор если упрощено это некий флаг, который есть у каждого объекта.
- Он нужен для избегания дата рейса, при блокировки на ключевом флаге/объекте

#### Что будет если без создания потоков, вызвать wait или notify?

- IllegalMonitorStateException

#### Какие есть проблемы многопоточности?

- [[Проблемы многопоточности]]

#### Чем CopyOnWriteArrayList отличается от обычно?

- Изменение: При каждой операции изменения (например, `add`, `set`, `remove`) создается новая копия массива, в то время как существующий массив остается неизменным для других потоков. Это минимизирует блокировки и обеспечивает безопасность чтения.
- Чтение: Чтение из `CopyOnWriteArrayList` происходит без блокировок, что делает его подходящим для сценариев с большим количеством операций чтения и небольшим количеством операций записи.

11. Какие существуют потокобезопасные коллекции, как они взаимодействуют с кучей

- ConcurrentHashMap: Использует сегментированные блокировки для обеспечения безопасности потоков и повышения производительности.
- CopyOnWriteArrayList: Создает копии внутреннего массива при каждом изменении, что делает его подходящим для частого чтения.
- BlockingQueue: Предоставляет потокобезопасные операции для добавления и удаления элементов, с блокировкой при выполнении операций.

12. Как осуществляется обработка ошибок в многопоточности?

Если упал один поток, он положил по Error всю программу, потому что потоки выполняются массивом потоков.

13. Что такое shutdown в контексте потоков?

Shutdown в контексте потоков обычно относится к завершению работы пула потоков или приложений, обеспечивая корректное завершение всех выполняемых задач. В Java это может быть выполнено с помощью методов `shutdown()` или `shutdownNow()` в `ExecutorService`. Эти методы:

- `shutdown()`: Останавливает прием новых задач и ожидает завершения уже запущенных.
- `shutdownNow()`: Прерывает текущие задачи и возвращает список ожидающих задач.

14. Какие преимущества и недостатки использования ExecutorService для управления потоками?

`ExecutorService` упрощает управление потоками, позволяя повторно использовать их из пула, что экономит ресурсы. Недостаток — необходимость корректного завершения, чтобы избежать утечек ресурсов.

15. Какие стратегии и подходы используются для оптимизации производительности при работе с потоками?

Оптимизация потоков включает использование пулов, минимизацию блокировок, замену синхронизации атомарными операциями и профилирование для выявления узких мест.

16. Как можно реализовать механизм ожидания завершения нескольких потоков?

Можно использовать `CountDownLatch` или `CyclicBarrier` для ожидания завершения нескольких потоков

17. Как можно создать поток, используя лямбда-выражения?

Поток можно создать с помощью лямбда-выражения, например, `new Thread(() -> { /* код */ }).start();`. Потому что Thread реализуется Runnable с одним абстрактным методом.

18. В чем преимущество использования интерфейса Callable по сравнению с Runnable при создании потоков?

`Callable` возвращает результат и может бросить исключение, чего не поддерживает `Runnable`, что делает `Callable` предпочтительным для задач, возвращающих значение.

19. Как реализовать паттерн Producer-Consumer с использованием многопоточности в Java?

В паттерне Producer-Consumer можно использовать `BlockingQueue`, где один поток производит данные, а другой их потребляет.

20. Какие основные состояния потоков в Java?

Основные состояния потока: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED.

![[Pasted image 20241212150415.png]]

21. Как работает метод interrupt() и в каких случаях он используется?

`interrupt()` прерывает поток, посылая ему сигнал прерывания. Используется, чтобы остановить поток корректно, например, при длительном ожидании.

22. Как реализовать безопасное взаимодействие между потоками при использовании общих ресурсов?

Для безопасного взаимодействия можно использовать синхронизацию, атомарные классы или блокировки (`ReentrantLock`) для избежания состояния гонки.

23. Каким образом можно реализовать паттерн «один производитель - много потребителей» в многопоточной программе на Java?

Для реализации паттерна «один производитель - много потребителей» можно использовать `BlockingQueue` или `ExecutorService` с несколькими потребителями.

24. Какой интерфейс из пакета java.util.concurrent представляет собой фреймворк, который упрощает выполнение асинхронных задач?

`CompletableFuture` — интерфейс из `java.util.concurrent` для асинхронных задач, который упрощает работу с потоками и их результатами.

25. В чем основное преимущество использования классов из пакета java.util.concurrent по сравнению с традиционным способом работы с потоками в Java?

Классы из `java.util.concurrent` предлагают более гибкие и высокоуровневые механизмы, чем `Thread` и `synchronized`, что упрощает разработку многопоточных приложений.

26. Какой класс из пакета java.util.concurrent используется для блокировок, которые позволяют более гибкое управление, чем синхронизированные блоки или методы?

`ReentrantLock` — класс, который предоставляет гибкую альтернативу для синхронизированных блоков, позволяя более точное управление блокировками.

27. Какой класс из пакета java.util.concurrent позволяет выполнить задачу в пуле потоков и вернуть результат в будущем?

`FutureTask` позволяет запустить задачу в пуле и вернуться к ней позже для получения результата.

28. Как работает ConcurrentHashMap и в чем его основное отличие от Hashtable и synchronizedMap?

`ConcurrentHashMap` обеспечивает лучшую производительность в многопоточной среде, сегментируя данные, в отличие от полной блокировки в `Hashtable` или `synchronizedMap`.
- [HashMap и ConcurrentHashMap популярные вопросы на собеседованиях](https://javarush.com/groups/posts/713-hashmap-i-concurrenthashmap-populjarnihe-voprosih-na-sobesedovanijakh)

29. Что такое CyclicBarrier и в каких сценариях его использование наиболее целесообразно?

`CyclicBarrier` используется для синхронизации фиксированного числа потоков, которые должны достигнуть общей точки перед продолжением.

![[Pasted image 20241212155833.png]]

30. В чем суть CAS в атомиках?

Имеллось ввиду что под капотом атомиков. Есть две переменные текущее, старое и новое значение. Старое это волотайл переменная, а текущее не волотайл, если они равны значит исполнить свап на новое.


31. Важно знать, как безопасно завершать потоки и почему использование Thread.stop() не рекомендуется.
- [Java. Остановись задача](https://habr.com/ru/articles/133413/)
- [Как правильно остановить поток? Для чего нужны методы .stop(), .interrupt(), .interrupted(), .isInterrupted().](https://telegra.ph/14-Kak-pravilno-ostanovit-potok-Dlya-chego-nuzhny-metody-stop-interrupt-interrupted-isInterrupted-01-12)

32. Потокобезопасные коллекции

[Concurrent Collections](Concurrent%20Collections.md)

#### Что такое JMM (Java Memory Model) и принцип Happens-Before?

- An unlock action on monitor `m` _happens-before_ all subsequent lock actions on `m`
- A write to a volatile variable `v` happens-before all subsequent reads of `v` by any thread
- The final action in a thread `T1` _happens-before_ any action in another thread `T2` that detects that `T1` has terminated.
- An action that starts a thread _happens-before_ the first action in the thread it starts.
- If thread `T1` interrupts thread `T2`, the interrupt by `T1` _happens-before_ any point where any other thread (including `T2`) determines that `T2` has been interrupted (by having an `InterruptedException` thrown or by invoking `Thread.interrupted` or `Thread.isInterrupted`).
- The write of the default value (`zero`, `false`, or `null`) to each variable _happens-before_ the first action in every thread.

- Не только освобождение монитора, но и все действия, идущие в порядке программы до освобождения, будут видны другому треду после захвата этого же монитора
- Не только запись в volatile поле, но и все действия, идущие в порядке программы до записи, будут видны другому треду после чтения этого же поля
- Не только финальное действие, но и все предыдущие действия треда T1 будут видны другому треду после завершения `T1.join()`
- Не только первое действие в треде, но и все последующие действия увидят дефолтную инициализацию переменной


#### Какие методы атомиков знаешь?

- [Методы](Atomic%20Type.md#Методы)

#### Что такое deadlock?

Когда два и более потоков вечно ожидают друг друга. _Deadlock имеет место быть, тогда и только тогда, когда диаграмма Холта, отражающая состояния процессов и ресурсов, **содержит цикл**._

![](Pasted%20image%2020250317144115.png)

How to Prevent Deadlocks

1. **Lock Ordering**: Always acquire locks in the same order.
2. **Timeouts**: Instead of waiting indefinitely, use timeouts.
3. **Thread Management**: Regularly monitor and manage thread activity to detect potential deadlocks.

#### Что такое livelock?

Livelock заключается в том, что потоки внешне как бы живут, но при этом не могут ничего сделать, т.к. условие, по которым они пытаются продолжить свою работу, не могут выполниться. По сути Livelock похож на deadlock, но только потоки не "зависают" на системном ожидании монитора, а что-то вечно делают.

Вы делаете телефонный звонок, но человек на другом конце тоже пытается вам позвонить. Вы оба повесите трубку и попробуйте снова через одно и то же время, что снова создаст такую же ситуацию. Это может продолжаться вечность.

How to Avoid Livelock

1. **Use Random Backoff**: Introduce randomness in decision making, allowing for one thread to proceed while the other waits.
2. **Resource Fairness**: Implement algorithms that guarantee all threads will acquire resources eventually.
#### Что такое Starvation?

От блокировок это явление отличается тем, что потоки не заблокированы, а им просто не хватает ресурсов на всех. Поэтому пока одни потоки на себя берут всё время выполнения, другие не могут выполниться. Starvation — это любая ситуация, когда параллельный процесс не может получить все ресурсы, необходимые для выполнения его работы.

У нас будет два работника. Один жадный(`greedyWorker`), другой вежливый(`politeWorker`). Обоим дается одинаковое кол-во времени на их полезную работу — спать по 3 наносекунде.

`greedyWorker` жадно удерживает общий ресурс(`sharedLock`) на протяжении всего цикла работы, тогда как politeWorker пытается блокировать его только тогда, когда это необходимо.

1. **Fair Scheduling**: Use synchronization mechanisms that ensure fair access to resources.
2. **Thread Prioritization**: Balance thread priority levels, avoiding scenarios where low-priority threads are completely overlooked.
3. **Resource Limits**: Establish limits for how long threads can hold resources.

#### Что такое race condition?

При работе с многопоточностью есть такое понятие, как "состояние гонки". Это явление заключается в том, что потоки делят между собой некоторый ресурс и код написан таким образом, что не предусматривает корректную работу в таком случае.

#### Completable Future vs Future

- **Future**: When using `Future`, you submit a task to an `ExecutorService`, and it returns a `Future` object. However, to get the result of the computation, **you must call the `get()` method**, which blocks the calling thread until the task is complete. This can make your application less responsive, as it waits for the computation to finish.  
- **CompletableFuture**: With `CompletableFuture`, you get non-blocking execution. It allows you to define what happens when the computation is done through methods like **`thenApply()`, `thenAccept()`, and** `**thenRun()**`. This makes your application much more responsive, as it doesn't block the main thread while waiting for the result.
- [Difference Between CompletableFuture And Future In Java](https://www.hungrycoders.com/blog/difference-between-completablefuture-and-future-in-java)

```java
// Using Future
Future<String> future = executor.submit(() -> {
    Thread.sleep(2000);
    return "Hello, Future!";
});
String result = future.get();  // Blocks until the result is ready

// Using CompletableFuture
CompletableFuture.supplyAsync(() -> {
    Thread.sleep(2000);
    return "Hello, CompletableFuture!";
}).thenAccept(System.out::println);  // Non-blocking
```

###### Расскажи про синхронное и асинхронное взаимодействие, и когда нужно выбирать тот или иной подход?


###### Многопоточность в Java, Thread, Runnable, Callable


###### CompletableFuture


###### Volatile, блок синхронизации


###### Executor service, виды, настройка


###### @Async


###### А если volatile использовать на Long - он станет атомарным?


###### Сама реализация в атомиках : где CAS алгоритм? как работает?


###### Мьютекс - что такое?


###### Блок Syncrhonized занят несколькими потоками - бывает ли такое? Рассказать про пример в виде Семафора, Phaser, CyclicBarrier


###### Типы потоков в Java. С начала 19-ой джавы появились новые потоки.

###### - виртуальные потоки

###### В чем преимущества вирутальных потоков?


###### NIO - рассказать для чего оно используется и чем отличается от обычного блокирующего ввода-вывода?


###### WebFlux - рассказать про реактивщину


###### Как в плане запроса понять, используется ли индекс?


###### Приходилось ли работать с представлениями бд?


###### Для чего нужны ключевые слова volatile и synchronized?


###### Какие синхронизаторы знаешь?

###### Разница между ConcurrentHashMap и SynchronizedHashMap?


## Resources

- [JDK concurrent package](https://habr.com/ru/articles/187854/)
- [Deadlocks, Livelocks и Starvation](https://ubiklab.net/posts/deadlocks-livelocks-starvation/)
- [Understanding Deadlock, Livelock, and Starvation in Java Concurrency](https://procodebase.com/article/understanding-deadlock-livelock-and-starvation-in-java-concurrency)
- [Difference Between CompletableFuture And Future In Java](https://www.hungrycoders.com/blog/difference-between-completablefuture-and-future-in-java)