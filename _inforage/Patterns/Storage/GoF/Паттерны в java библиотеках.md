# [[03 Основное/GoF Одиничка]]

## **`java.lang.Runtime`**

- Предоставляет доступ к среде выполнения Java, и экземпляр этого класса всегда уникален для приложения.

```java
Runtime runtime = Runtime.getRuntime(); 
runtime.gc(); // вызвать сборщик мусора
```

# Observer, он же [[03 Основное/GoF Наблюдатель]]

## Deprecated: `java.util.Observer` и `java.util.Observable`

- Эти классы используются для реализации паттерна Observer, где `Observable` уведомляет своих `Observers` об изменениях. Содержит внутри Vector<\Observer\> obs;
## java.util.concurrent.Flow

- Замена Observer, Observable  реализующая интерфейсы Publisher и Subscriber.

# [[03 Основное/Gof Стратегия]]

## `java.util.Comparator`

- Используется для реализации паттерна Strategy, где различные стратегии сортировки могут быть переданы в метод `Collections.sort()`.

# [[03 Основное/GoF Декоратор]]

## `java.io.BufferedInputStream`
 
- Является декоратором для других классов ввода-вывода, который добавляет функциональность буферизации.

```java
FileInputStream fileInputStream = new FileInputStream("file.txt");
BufferedInputStream bufferedInputStream = new BufferedInputStream(fileInputStream);
```

# [[03 Основное/GoF Строитель]]

## **`StringBuilder`** и **`StringBuffer`** 

- Строят строки по шагам.

## **`java.nio.ByteBuffer`** 

- Используется для создания буфера, поддерживая методы для удобного добавления данных.

# [[03 Основное/GoF Команда]]

## **`java.lang.Runnable`** 

- Позволяет инкапсулировать кусок логики в объект и передавать его потоку для выполнения.


