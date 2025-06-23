#### Задача 1

```java
/*
Что выведеться в обоих вариантах и почему?
*/

public static void main(String[] args) {  
    List<String> key = new ArrayList<>(List.of("a", "b", "c", "d", "e"));  
  
    Map<List<String>, String> map = new HashMap<>();  
    map.put(key, "Hello");  
  
    key.add("f");  
    System.out.println(map.get(key));  
    System.out.println(map);
} 

public static void main(String[] args) {  
    List<String> key = new ArrayList<>(List.of("a", "b", "c", "d", "e"));  
  
    Map<List<String>, String> map = new HashMap<>();  
    map.put(key, "Hello");  
  
    map.remove(key);  
    key.add("f");  
    map.put(key, "Hello");  
    System.out.println(map.get(key));  
    System.out.println(map);  
}
```

#### Задача 2


```java
@Service
public class DequeTask {  
  
    private static Deque<String> deque = new ArrayDeque<>();  
    public void method () {  
        // какая-то логика работы с deque, например, описанная ниже:  
        deque.add("string");  
        deque.add("string2");  
        System.out.println(String.join(", ", deque));  
        deque.removeLast();  
        deque.removeLast();  
    }
}    
```


#### Задача 3

Что выведет на консоль следующий код

```java
List<Integer> list = List.of(1, 2, 3);
ListIterator<Integer> listIterator = list.listIterator();
System.out.println(listIterator.previous());
```


#### Задача 4

Что будет в результате вызова?

```java
public static void main(String[] args) {  
    Map<String, String> map = new HashMap<>();  
    map.put("a", "a");  
  
    for (String key : map.keySet()) {  
        map.remove(key);  
    }  
}
```