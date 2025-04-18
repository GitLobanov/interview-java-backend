- Строгая типизация
- Единая реализация
    - Параметрический полиморфизм
- Отсутствие информации о типе

- Раньше
```java
Stack stack = new ArrayStack();
stack.push(1);
Integer i = (Integer) stack.pop();
```
- Теперь
```java
Stack<Integer> stack = new ArrayStack<Integer>();
stack.push(1);
Integer i = stack.pop();
```
- Java 7+ `Stack<Integer> stack = new ArrayStack<>();`

# Ограничения

- Дженерики ИНВАРИАНТНЫ. 
`Запись List<Number> list = new ArrayList<Integer>() не скомпилируется.`  
- Нельзя инициализировать примитивами  
- Нельзя типизировать статические поля  
- Нельзя создать объект типа дженерика (T obj = new T())  
- Нельзя создать массив типа дженерика  
- Нельзя параметризировать классы иерархии Throwable, анонимные классы и Enum.
- Но, можно сделать extends от исключений:
```java
interface Reader<T, E extends Exception> {
	T next() throws E;
	boolean hasNext() throws E;
}
```

# Копаем
## Стирание информации о типах

From

```java
List<String> list = new ArrayList<>(…);
String max = list.get(0);
for (int i = 1; i < list.size(); i++) {
    String next = list.get(i);
    if (next.compareTo(max) > 0) {
        max = next;
    }
}
```

To

```java
List list = new ArrayList(…);
String max = (String) list.get(0);
for (int i = 1; i < list.size(); i++) {
    String next = (String) list.get(i);
    if (next.compareTo(max) > 0) {
        max = next;
    }
}
```

## Generic – один класс

```java
Stack<String> ss = new AS<String>();
Stack<Integer> si = new AS<Integer>();
ss.getClass() == si.getClass() // True

ss instanceof AS // True
ss instanceof AS<String> // Запрещено
```

## Преобразование типов

- Уничтожение информации о типе
    - `Stack s = new ArrayStack<String>();`
- Добавление информации о типе
    - `Stack<String> s = (Stack<String>) new ArrayStack();`
    - `Stack<String> s1 = new ArrayStack();`
- Unchecked warning
    - Ответственность программиста
    - `@SuppressWarnings("unchecked")`
    - `Stack<String> s1 = new ArrayStack();`
# Проблемы
## Heap pollution

Ситуация, когда из-за сырых типов и из-за затирания типов в коллекции, которая параметризована определенным типом, лежат объекты совершенно другого типа, что приводит к проблемам каста при работе с данными коллекциями.

# Ресурсы

- [Полиморфизм и Generics](https://www.kgeorgiy.info/courses/paradigms/slides/generics.xhtml#(31))