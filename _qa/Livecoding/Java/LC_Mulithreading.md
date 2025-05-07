#### Задача 1

Напишите программу, которая создает и запускает поток, который выводит числа от 1 до 10, с задержкой в 1 секунду между выводами.

#### Задача 2

Напишите программу, которая создает два потока, которые одновременно увеличивают счетчик на 1000. 

```java
public class AtomicCounterExample {
    private static final AtomicInteger counter = new AtomicInteger(0);
    private static final int INCREMENTS_PER_THREAD = 1000;

    public static void main(String[] args) throws InterruptedException {
        Thread thread1 = new Thread(new IncrementTask());
        Thread thread2 = new Thread(new IncrementTask());

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final counter value: " + counter.get());
    }

    static class IncrementTask implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < INCREMENTS_PER_THREAD; i++) {
                counter.incrementAndGet();
            }
        }
    }
}
```

#### Задача 3

Напиши DoubleCheckSingleton

```java
class DoubleCheckedLockingSingleton {
    private static volatile DoubleCheckedLockingSingleton instance;

    static DoubleCheckedLockingSingleton getInstance() {
        DoubleCheckedLockingSingleton current = instance;
        if (current == null) {
            synchronized (DoubleCheckedLockingSingleton.class) {
                current = instance;

                if (current == null) {
                    instance = current = new DoubleCheckedLockingSingleton();
                }
            }
        }
        return current;
    }
}
```

#### Задача 4

Написать deadlock

```java
public class TestDeadlockExample1 {
    public static void main(String[] args) {
        final String resource1 = "resource1";
        final String resource2 = "resource2";

        // t1 tries to lock resource1 then resource2
        Thread t1 = new Thread() {
            @SneakyThrows
            public void run() {
                synchronized (resource1) {
                    System.out.println("Thread 1: locked resource 1");

                    Thread.sleep(100);

                    synchronized (resource2) {
                        System.out.println("Thread 1: locked resource 2");
                    }
                }
            }
        };

        // t2 tries to lock resource2 then resource1
        Thread t2 = new Thread() {
            @SneakyThrows
            public void run() {
                synchronized (resource2) {
                    System.out.println("Thread 2: locked resource 2");

                    Thread.sleep(100);

                    synchronized (resource1) {
                        System.out.println("Thread 2: locked resource 1");
                    }
                }
            }
        };

        t1.start();
        t2.start();
    }
}
```

#### Задача 5

Напишите минимальный неблокирующий стек (всего два метода — `push()` и `pop()`).

```java
class NonBlockingStack<T> {
    private final AtomicReference<Element> head = new AtomicReference<>(null);

    NonBlockingStack<T> push(final T value) {
        final Element current = new Element();
        current.value = value;
        Element recent;
        do {
            recent = head.get();
            current.previous = recent;
        } while (!head.compareAndSet(recent, current));
        return this;
    }

    T pop() {
        Element result;
        Element previous;
        do {
            result = head.get();
            if (result == null) {
                return null;
            }
            previous = result.previous;
        } while (!head.compareAndSet(result, previous));
        return result.value;
    }

    private class Element {
        private T value;
        private Element previous;
    }
}
```

#### Напишите минимальный неблокирующий стек (всего два метода — `push()` и `pop()`) с использованием `Semaphore`.

```java
class SemaphoreStack<T> {
    private final Semaphore semaphore = new Semaphore(1);
    private Node<T> head = null;

    SemaphoreStack<T> push(T value) {
        semaphore.acquireUninterruptibly();
        try {
            head = new Node<>(value, head);
        } finally {
            semaphore.release();
        }

        return this;
    }

    T pop() {
        semaphore.acquireUninterruptibly();
        try {
            Node<T> current = head;
            if (current != null) {
                head = head.next;
                return current.value;
            }
            return null;
        } finally {
            semaphore.release();
        }
    }

    private static class Node<E> {
        private final E value;
        private final Node<E> next;

        private Node(E value, Node<E> next) {
            this.value = value;
            this.next = next;
        }
    }
}
```

#### Напишите минимальный неблокирующий ArrayList (всего четыре метода — `add()`, `get()`, `remove()`, `size()`).

```java
class NonBlockingArrayList<T> {
    private volatile Object[] content = new Object[0];

    NonBlockingArrayList<T> add(T item) {
        return add(content.length, item);
    }

    NonBlockingArrayList<T> add(int index, T item) {
        if (index < 0) {
            throw new IllegalArgumentException();
        }
        boolean needsModification = index > content.length - 1;
        if (!needsModification) {
            if (item == null) {
                needsModification = content[index] != null;
            } else {
                needsModification = !item.equals(content[index]);
            }
        }
        if (needsModification) {
            final Object[] renewed = Arrays.copyOf(content, Math.max(content.length, index + 1));
            renewed[index] = item;
            content = renewed;
        }
        return this;
    }

    NonBlockingArrayList<T> remove(int index) {
        if (index < 0 || index >= content.length) {
            throw new IllegalArgumentException();
        }
        int size = content.length - 1;
        final Object[] renewed = new Object[size];
        System.arraycopy(content, 0, renewed, 0, index);
        if (index + 1 < size) {
            System.arraycopy(content, index + 1, renewed, index, size - index);
        }
        content = renewed;
        return this;
    }

    T get(int index) {
        return (T) content[index];
    }

    int size() {
        return content.length;
    }
}
```

#### Напишите потокобезопасную реализацию класса с неблокирующим методом `BigInteger next()`, который возвращает элементы последовательности: `[1, 2, 4, 8, 16, ...]`.

```java
class PowerOfTwo {
    private AtomicReference<BigInteger> current = new AtomicReference<>(null);
    
    BigInteger next() {
        BigInteger recent, next;
        do {
            recent = current.get();
            next = (recent == null) ? BigInteger.valueOf(1) : recent.shiftLeft(1);
        } while (!current.compareAndSet(recent, next));
        return next;
    }
}
```

#### Напишите простейший многопоточный ограниченный буфер с использованием `synchronized`.

```java
class QueueSynchronized<T> {
    private volatile int size = 0;
    private final Object[] content;
    private final int capacity;

    private int out;
    private int in;

    private final Object isEmpty = new Object();
    private final Object isFull = new Object();

    QueueSynchronized(final int capacity) {
        this.capacity = capacity;
        content = new Object[this.capacity];
        out = 0;
        in = 0;
        size = 0;
    }

    private int cycleInc(int index) {
        return (++index == capacity)
                ? 0
                : index;
    }

    @SuppressWarnings("unchecked")
    T get() throws InterruptedException {
        if (size == 0) {
            synchronized (isEmpty) {
                while (size < 1) {
                    isEmpty.wait();
                }
            }
        }
        try {
            synchronized (this) {
                final Object value = content[out];
                content[out] = null;
                if (size > 1) {
                    out = cycleInc(out);
                } 
                size--;
                return (T) value;
            }
        } finally {
            synchronized (isFull) {
                isFull.notify();
            }
        }
    }

    QueueSynchronized<T> put(T value) throws InterruptedException {
        if (size == capacity) {
            synchronized (isFull) {
                while (size == capacity) {
                    isFull.wait();
                }
            }
        }
        synchronized (this) {
            if (size == 0) {
                content[in] = value;
            } else {
                in = cycleInc(in);
                content[in] = value;
            }
            size++;
        }
        synchronized (isEmpty) {
            isEmpty.notify();
        }
        return this;
    }
}
```

#### Напишите простейший многопоточный ограниченный буфер с использованием `ReentrantLock`.

```java
class QueueReentrantLock<T> {

    private volatile int size = 0;
    private final Object[] content;
    private final int capacity;

    private int out;
    private int in;

    private final ReentrantLock lock = new ReentrantLock();
    private final Condition isEmpty = lock.newCondition();
    private final Condition isFull = lock.newCondition();

    QueueReentrantLock(int capacity) {
        try {
            lock.lock();
            this.capacity = capacity;
            content = new Object[capacity];
            out = 0;
            in = 0;
        } finally {
            lock.unlock();
        }
    }

    private int cycleInc(int index) {
        return (++index == capacity)
                ? 0
                : index;
    }

    @SuppressWarnings("unchecked")
    T get() throws InterruptedException {
        try {
            lock.lockInterruptibly();
            if (size == 0) {
                while (size < 1) {
                    isEmpty.await();
                }
            }
            final Object value = content[out];
            content[out] = null;
            if (size > 1) {
                out = cycleInc(out);
            }
            size--;
            isFull.signal();
            return (T) value;
        } finally {
            lock.unlock();
        }
    }

    QueueReentrantLock<T> put(T value) throws InterruptedException {
        try {
            lock.lockInterruptibly();
            if (size == capacity) {
                while (size == capacity) {
                    isFull.await();
                }
            }
            if (size == 0) {
                content[in] = value;
            } else {
                in = cycleInc(in);
                content[in] = value;
            }
            size++;
            isEmpty.signal();
        } finally {
            lock.unlock();
        }
        return this;
    }
}
```