`CyclicBarrier` — это синхронизатор в Java, который позволяет группе потоков ждать друг друга, чтобы достичь определённого состояния (ближайшему barrier.await()), прежде чем продолжить выполнение. В отличие от `CountDownLatch`, который может использоваться только один раз, `CyclicBarrier` может использоваться повторно, что делает его полезным для задач, которые повторяются в циклах.

### Основные методы CyclicBarrier

1. **`CyclicBarrier(int parties)`**: Конструктор, создающий барьер для указанного числа потоков.
    
2. **`CyclicBarrier(int parties, Runnable barrierAction)`**: Конструктор, создающий барьер для указанного числа потоков и задающий действие, которое будет выполнено, когда все потоки достигнут барьера.
    
3. **`int await()`**: Этот метод блокирует текущий поток до тех пор, пока не все потоки не достигнут барьера. Когда все потоки достигли барьера, они все одновременно освобождаются и продолжают выполнение.
    
4. **`int await(long timeout, TimeUnit unit)`**: Этот метод блокирует текущий поток до достижения барьера или до истечения указанного времени ожидания.
    
5. **`boolean isBroken()`**: Этот метод возвращает `true`, если барьер был сломан (например, если один из потоков был прерван, ожидая у барьера).
    
6. **`void reset()`**: Этот метод сбрасывает барьер, чтобы его можно было использовать снова, если он был сломан.
