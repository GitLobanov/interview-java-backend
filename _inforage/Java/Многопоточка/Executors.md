`ExecutorService` пришел на замену `new Thread(runnable)` чтобы упростить работу с потоками. `ExecutorService` помогает повторно использовать освободившиеся потоки, организовывать очереди из задач для пула потоков, подписываться на результат выполнения задачи. Вместо интерфейса `Runnable` пул использует интерфейс `Callable` (умеет возвращать результат и кидать ошибки).

В настоящее время мало кто создает потоки вручную, потому что это неудобно и накладно. В пакете concurrent представлен интерфейс ExecutorService и его реализации, которые являются пулами потоков и имеют следующие преимущества:  

- сам создает потоки, когда это необходимо  
- удаляет неиспользуемые потоки  
- переиспользует потоки


newSingleThreadPool() - создает пулл с 1 потоком
newFixedThreadPool(int number) - создает пулл с указанным количеством потоков  
newCachedThreadPool() - создает динамический пулл потоков
newScheduledThreadPool() - создает фиксированный пулл потоков, который может выполнять задачки по расписанию  
newWorkStealingPool() - создает ForkJoinPool

![](../../../_res/Pasted%20image%2020250110141003.png)


ThreadPoolExecutor

Пул потоков с возможностью указывать рабочее и максимальное кол-во потоков в пуле, очередь для задач.

ScheduledThreadPoolExecutor

Расширяет функционал ThreadPoolExecutor возможностью выполнять задачи отложенно или регулярно.

ThreadPoolExecutor

Более легкий пул потоков для «самовоспроизводящих» задач. Пул ожидает вызовов `fork()` и `join()` методов у дочерних задач в родительской.
