#### Задача 1

Будет ли OutOfMemoryError?

```java
public static void main(String[] args) {
    Map<Long, String> innnerMap = new HashMap<>();  
    Random random = new Random();  
    for (long i = 0; i < 1_000_000_000_00L; i++) {  
        innnerMap.put(i, Integer.toString(random.nextInt(1, 100)).intern());  
    }  
}
```