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
    public static void task2() {
        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5)
                .filter(i -> i % 2 != 0);

        System.out.println(stream.reduce(5, Integer::sum));

        // 14
    }
```