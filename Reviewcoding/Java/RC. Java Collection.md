#### RC 1

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

#### RC 2

