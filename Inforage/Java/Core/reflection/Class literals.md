---
Определение: 
Примечание:
---
```java
//тут всё понятно
Class<String> c1 = String.class;
Class<Integer> c2 = Integer.class;

//Да, так можно!!
Class<Integer> c3 = int.class;
//А ещё double.class, boolean.class и т. д.

//При этом c2 и с3 -- существенно РАЗНЫЕ объекты
//(например, c3.getConstructors() возвращает пустой массив).

//Тут ничего неожиданного нет, массивами можно параметризовать:
Class<int[]> c4 = int[].class;
```