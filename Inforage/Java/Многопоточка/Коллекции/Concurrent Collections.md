###### ArrayBlockingQueue
Честная очередь для передачи сообщения из одного потока в другой. Поддерживает блокирующие (`put()` `take()`) и неблокирующие (`offer()` `pool()`) методы. Запрещает null значения. Емкость очереди должна быть указанна при создании.  
#### [ConcurrentHashMap](ConcurrentHashMap.md)
###### ConcurrentSkipListMap
Сбалансированная многопоточная ключ-значение структура (O(log n)). Поиск основан на списке с пропусками. Карта должна иметь возможность сравнивать ключи.  
###### ConcurrentSkipListSet
`ConcurrentSkipListMap` без значений.  
###### CopyOnWriteArrayList
Блокирующий на запись, не блокирующий на чтение список. Любая модификация создает новый экземпляр массива в памяти.  
###### CopyOnWriteArraySet
`CopyOnWriteArrayList` без значений.  
###### DelayQueue
`PriorityBlockingQueue` разрешающая получить элемент только после определенной задержки (задержка объявляется через `Delayed` интерфейс объекта). `DelayQueue` может быть использована для реализации планировщика. Емкость очереди не фиксирована.  
###### LinkedBlockingDeque
Двунаправленная `BlockingQueue`, основанная на связанности (cache-miss & cache coherence overhead). Емкость очереди не фиксирована.  
###### LinkedBlockingQueue
Однонаправленная `BlockingQueue`, основанная на связанности (cache-miss & cache coherence overhead). Емкость очереди не фиксирована.  
###### LinkedTransferQueue
Однонаправленная `BlockingQueue`, основанная на связанности (cache-miss & cache coherence overhead). Емкость очереди не фиксирована. Данная очередь позволяет ожидать когда элемент «заберет» обработчик.  
###### PriorityBlockingQueue
Однонаправленная `BlockingQueue`, разрешающая приоритизировать сообщения (через сравнение элементов). Запрещает null значения.  
###### SynchronousQueue
Однонаправленная `BlockingQueue`, реализующая `transfer()` логику для `put()` методов.