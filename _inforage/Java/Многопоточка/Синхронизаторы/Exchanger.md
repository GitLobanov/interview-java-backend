---
Определение: Может понадобиться, для того, чтобы обменяться данными между двумя потоками в определенной точки работы обоих потоков. Обменник — обобщенный класс, он параметризируется типом объекта для передачи.
Примечание: Стоит отметить, что обменник поддерживает передачу null значения. Это дает возможность использовать его для передачи объекта в одну сторону, или, просто как точку синхронизации двух потоков.
---
### Методы

Обменник является точкой синхронизации пары потоков: поток, вызывающий у обменника метод **exchange()** блокируется и ждет другой поток. Когда другой поток вызовет тот же метод, произойдет обмен объектами: каждая из них получит аргумент другой в методе **exchange()**.

### Примеры

![[../../../../_res/exchanger.gif]]