#### Задача 1

```java  
/*  
    Каким будет результат работы следующего кода?  
 */  
public abstract class Test implements Runnable {  
  
    private Object lock = new Object();  
  
    public void lock() {  
        synchronized (lock) {  
            try {  
                lock.wait();  
                System.out.println("1");  
            } catch (InterruptedException e) {  
            }  
        }  
    }  
  
    public void unlock() {  
        synchronized (lock) {  
            lock.notify();  
            System.out.println("2");  
        }  
    }  
  
    public static void main(String s[]) {  
  
        new Thread(new Test() {  
            public void run() {  
                lock();  
            }  
        }).start();  
        new Thread(new Test() {  
            public void run() {  
                unlock();  
            }  
        }).start();  
    }  
}  
/*  
        lock.wait(1000);   
          
        while (!condition) {  
            lock.wait();  
        }  
          
        Thread.currentThread().interrupt();  
          
        private static final ReentrantLock lock = new ReentrantLock();  
        private static final Condition cond = lock.newCondition();  
 */  
```  
#### Задача 2
  
```java  
  
///Что будет выведено на экран?  
  
public class MeaningOfLife{  
  
    private final ExecutorService executor = Executors.newFixedThreadPool(10);  
      
    Future <Integer> findElseWhere(Integer currentAnswer){  
        return executor.submit(() -> {  
            Thread.sleep(1000L);  
            return currentAnswer + 6;  
        });  
    }  
  
    CompletebleFuture exploreDifferentMeaning (Integer currentAnswer) {  
        return CompletebleFuture.supplyAsync(() -> {  
            try{  
                Thread.sleep(2000L);  
            }catch(InterruptedException e){  
                throw new RuntimeException(e);  
            }  
            return currentAnswer + 10;  
        });  
    }  
  
    static void announceAnswer(Integer answer){   
        System.out.println(answer);  
    }  
  
    public static void main(String[] args) throws ExecutionException, InterruptedException{  
        Integer answer = 42;  
        MeaningOfLife meaningOfLife = new MeaningOfLife();  
        Future<Integer> elseWhereAnswer = meaningOfLife.findElsewhere(answer);  
        meaningOfLife.exploreDifferentMeaning(answer).thenAccept(MeaningOfLife::announceAnswer);  
        announceAnswer(elsewhereAnswer.get());  
        announceAnswer(elsewereAnswer.get());  
        announceAnswer(answer);  
    }  
}  
```  

#### Задача 3

```java  
public class StopThreadTest {  
    private static Boolean stopRequested;  
  
    public static void main(String[] args) throws Exception {  
        Thread backgroundThread = new Thread(() -> {  
      
            int i = 0;  
            while (!stopRequested) {  
                i++;  
            }  
        });  
      
        backgroundThread.start();  
        TimeUnit.SECONDS.sleep(1);  
        stopRequested = true;  
  
    }  
}  
```

#### Задача 4


```java  
//Код-ревью: что будет выведено в консоль ?  
  
public class Test {  
    private Object object = new Object();  
  
    public void print() throws InterruptedException {  
  
        CompletableFuture.runAsync(() -> {  
            synchronized (object) {  
                try {  
                    Thread.sleep(10000);  
                } catch (InterruptedException e) {  
                    e.printStackTrace();  
                }  
                System.out.println("1");  
            }  
        });  
  
        Thread.sleep(100);  
  
        object = new Object();  
  
        CompletableFuture.runAsync(() -> {  
            synchronized (object) {  
                System.out.println("2");  
            }  
        });  
  
    }  
}  
```  


#### Задача 5

какие способы знаешь улучшения кода? замена структуры хранения в боле конкрентную в многопоточной среде, также getFirst() изменяться будет очень редко, нужно понять что здесь можно сделать

```java
public class ThreadSafeList {
  private List<Integer> numbers = new ArrayList<>();
  public synchronized void add(Integer number) {
     numbers.add(number);
  }
  public synchronized Integer getFirst() {
    if (numbers.size() > 0) {
        return numbers.get(0);
    }
    return null;
  }
}
```

- `private final List<Integer> numbers = new CopyOnWriteArrayList<>();`
- Кэширование первого элемента (volatile или атомик)
- double check locking (synchronized, lock, semaphore)
- Использование Immutable Collections
- lock Condition

```java
public class ThreadSafeList {
    private final List<Integer> numbers = new ArrayList<>();
    private final ReentrantReadWriteLock lock = new ReentrantReadWriteLock();
    private final Condition notEmpty = lock.writeLock().newCondition();

    public void add(Integer number) {
        lock.writeLock().lock();
        try {
            numbers.add(number);
            notEmpty.signalAll(); // Уведомляем ожидающие потоки
        } finally {
            lock.writeLock().unlock();
        }
    }

    public Integer getFirst() {
        lock.readLock().lock();
        try {
            while (numbers.isEmpty()) {
                lock.readLock().unlock();
                lock.writeLock().lock();
                try {
                    notEmpty.await(); // Ждем, пока список не станет непустым
                } finally {
                    lock.readLock().lock();
                    lock.writeLock().unlock();
                }
            }
            return numbers.get(0);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            throw new RuntimeException(e);
        } finally {
            lock.readLock().unlock();
        }
    }
}
```