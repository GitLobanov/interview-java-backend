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
-------------------------------------  
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