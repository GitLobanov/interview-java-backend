#### Задача 1

```java  
/*  
Что выведется?  
*/  
public class Main {  
  
    public static void main(String[] args) {  
  
        Set<Person> persons = new HashSet<>();  
  
        persons.add(new Person("alex", "2"));  
        persons.add(new Person("sergei", "3"));  
        persons.add(new Person("alex", "2"));  
  
        var streamPerson = persons.stream();  
        System.out.println(streamPerson  
                .map(Person::getName)  
                .collect(Collectors.joining(", "))  
        );  
  
        System.out.println(streamPerson  
                .map(Person::getAge)  
                .collect(Collectors.toList())  
        );  
  
    }  
  
    public static class Person {  
        private final String name;  
        private final String age;  
  
        public Person(String name, String age) {  
            this.age = age;  
            this.name = name;  
        }  
  
        public String getName() {  
            return name;  
        }  
  
        public String getAge() {  
            return age;  
        }  
    }  
}  
```  
  
проверка: StreamApiPerson  
#### Задача 2

```java  
/*  
Что не так?  
 */  
public static void main(String[] args) {  
    List<StringBuilder> list = Arrays.asList(new StringBuilder("Java"), new StringBuilder("Hello"));  
    list.stream().map((x) -> x.append("World"));  
    list.forEach(System.out::print);  
}  
```  

#### Задача 3

```java  
public static void main(String[] args) {  
    int b = (int) Stream.of(1,3,6,7)  
            .filter(a-> a >5)  
            .peek(a-> System.out.println(a))  
            .toList().stream()  
            .peek(a-> System.out.println(a))  
            .count();  
    System.out.println(b);  
}  
  
// Ответ: 6,7,2. Второй peek() не отработает, так как не изменяет ничего   
// и будет оптимизирован JVM, потому что и так вызывается count().  
  
```  

Проверка: PeekStreamPeek  

#### Задача 4
  
```java  
public static void main(String[] args) {  
    List.of("A1", "A2", "B1", "B2").stream()  
            .peek(System.out::println)  
            .sorted()  
            .peek(System.out::println)  
            .filter(it -> it.startsWith("A"))  
            .forEach(System.out::println);  
}  
```

#### Задача 5

```java
// Какой будет вывод?
Stream.of(1,2,3,4)
        .filter(i -> i % 2 == 0);
        .peek(sout);
```

#### Задача 6

 Что будет выведено

```java
    public static void task1() {
        Stream.of("d2", "a2", "b1", "b3", "c", "b1")
                .map(s -> {
                    System.out.println("map: " + s);
                    return s.toUpperCase();
                })
                .distinct()
                .anyMatch(s -> {
                    System.out.println("anyMatch: " + s);
                    return s.startsWith("A2");
                });
    }
```

#### Задача 7

```java
// Что будет в результате?

public static void task2() {
	Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5)
        .filter(i -> i % 2 != 0);
    System.out.println(stream.reduce(5, Integer::sum));       
}
```

#### Задача 8

```java
private static void task1 () {
	Stream<Integer> stream1 = Stream.of(1, 2, 3, 4, 5).map(i -> ++i);
	Stream<Integer> stream2 = stream.peek(System.out::println);
	List<Integer> result = stream1.toList();
}
```

#### Задача 9

```java
private static void task2() {  
    IntStream.rangeClosed(1, 4)  
            .peek(System.out::println)  
            .skip(2)  
            .forEach(System.out::println);  
}
```

#### Задача 10

```java
private static void task3() {  
    List<Integer> list = new ArrayList<>(java.util.List.of(1, 2, 3));  
    List<Integer> result = list.stream()  
            .peek(list::add)  
            .toList();  
    System.out.println(result);  
}
```

#### Задача 11

```java
private static void task() {  
    Stream.iterate(0, n -> n + 1)  
            .filter(n -> n % 2 == 0)  
            .map(n -> n * 10)  
            .sorted()  
            .limit(3)  
            .forEach(System.out::println);  
}
```

#### Задача 12

```java
// В каком порядке выведеться результат?
private static void task4() {
	List.of (1,2,3,4,5).stream().parallel().forEach (System.out::println);
	List.of(1,2,3,4,5).stream().parallel().forEachOrdered (System.out::println);
	Set.of (1,2,3,4,5).stream().parallel().forEach(System.out::println);
	Set.of (1,2,3,4,5).stream().parallel().forEachOrdered (System.out::println);
}
```

