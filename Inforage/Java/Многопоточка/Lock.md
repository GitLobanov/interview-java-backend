Lock - интерфейс
ReentrantLock реализация для Lock
ReadWriteLock - интерфейс (несет два Lockа под капотом), на и ReentrantReadWriteLock реализация

The ReentrantReadWriteLock maintains two separate locks, one for reading and one for writing:

`Lock readLock = rwLock.readLock();`
`Lock writeLock = rwLock.writeLock();`

Then you can use the read lock to safeguard a code block that performs read operation like this:

```java
readLock.lock();
 
try {
    // reading data
} finally {
 
    readLock.unlock();
}
```

And use the write lock to safeguard a code block that performs update operation like this:

```java
writeLock.lock();
 
try {
    // update data
} finally {
 
    writeLock.unlock();
}
```

A ReadWriteLock implementation guarantees the following behaviors:

- Multiple threads can read the data at the same time, as long as there’s no thread is updating the data.
- Only one thread can update the data at a time, causing other threads (both readers and writers) block until the write lock is released.
- If a thread attempts to update the data while other threads are reading, the write thread also blocks until the read lock is released.

So ReadWriteLock can be used to add concurrency features to a data structure, but it doesn’t guarantee the performance because it depends on various factors: how the data structure is designed, the contention of reader and writer threads at real time, CPU architecture (single core  or multicores), etc.

Similar to ReentrantLock, the ReentrantReadWriteLock allows a thread to acquire the read lock or write lock multiple times recursively, thus the word “Reentrant”.

Let’s see an example that adds concurrency features to an ArrayList using ReentrantReadWriteLock. Consider the following data structure:

```java
public class ReadWriteList<E> {
    private List<E> list = new ArrayList<>();
    private ReadWriteLock rwLock = new ReentrantReadWriteLock();
 
    public ReadWriteList(E... initialElements) {
        list.addAll(Arrays.asList(initialElements));
    }
 
    public void add(E element) {
        Lock writeLock = rwLock.writeLock();
        writeLock.lock();
 
        try {
            list.add(element);
        } finally {
            writeLock.unlock();
        }
    }
 
    public E get(int index) {
        Lock readLock = rwLock.readLock();
        readLock.lock();
 
        try {
            return list.get(index);
        } finally {
            readLock.unlock();
        }
    }
 
    public int size() {
        Lock readLock = rwLock.readLock();
        readLock.lock();
 
        try {
            return list.size();
        } finally {
            readLock.unlock();
        }
    }
 
}
```


## Resources

- [Java ReadWriteLock and ReentrantReadWriteLock Example](https://www.codejava.net/java-core/concurrency/java-readwritelock-and-reentrantreadwritelock-example)
- [Boosting Java Performance: Mastering ReadWriteLock](https://javanexus.com/blog/java-performance-optimization-mastering-readwritelock)
