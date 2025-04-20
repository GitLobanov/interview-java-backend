#### RC 1

```java  
/*
Что будет результатом
*/
public class Solution {  
    public static void main(String[] args) {  
        Integer a = 0;  
        int b = 0;  
        AtomicInteger c = new AtomicInteger(0);  
        String d = "0";  
        increment(a);
        increment(b);
        increment(c);
        increment(d);
        System.out.println(a);  
        System.out.println(b);  
        System.out.println(c);  
        System.out.println(d);  
    }  
  
    public static void increment(Integer a) {  
        ++a;  
    }  
  
    public static void increment(int b) {  
        ++b;  
    }  
  
    public static void increment(AtomicInteger c) {  
        c.incrementAndGet();  
    }  
  
    public static void increment(String d) {  
        d = String.valueOf(  
                Integer.parseInt(d) + 1  
        );  
    }  
}      
```  
  
Проверка: IncrementDifference  
  
#### RC 2
  
```java  

/*
Что будет результатом?
*/
public class Main {  
    public static void main(String[] args) {  
  
        Map<SomeKey, string> test = new HashMap<>();  
        SomeKey key1 = new SomeKey("firstKey");  
        SomeKey key2 = new SomeKey("secondKey");  
        test.put(key1, "firstValue");  
        test.put(key2, "secondValue");  
        System.out.println(test.get(key1));  
        SomeKey key3 = new SomeKey("secondKey");  
        System.out.println(test.get(key3));  
        key2.value  
        System.out.println(test.get(key2));  
    }  
}                  
  
    class SomeKey {  
  
        public String value;  
  
        SomeKey(String val) {  
            this.value = val;  
        }  
  
        public int hashCode() {  
            return 1;  
        }  
  
        public boolean equals(Object obj) {  
            if (obj instanceof SomeKey) {  
                return value.equals(((SomeKey) obj).value);  
            }  
            return false;  
        }  
}      
```  

#### RC 3
  
```java
/*
Что не так?
Если нет, как исправить?
*/
public static void main(string[] args) {  
    List<? extends Fruit> fruits = new ArrayList<>();  
    fruits.add(new Apple());  
    fruits.add(new Orange());  
}  
  
class Fruit {}  
class Apple extends Fruit {}  
class Orange extends Fruit {}  
```  

#### RC 4
  
```java  
abstract class Camel {  
    void travel();  
}  
  
interface EatsGrass {  
    protected int chew();  
}  
  
abstract class Elephant {  
    private abstract class SleepsALot {  
        abstract int sleep();  
    }  
}  
  
class Eagle {  
    abstract fly();  
}  
```  
  
#### RC 5
  
```java  
interface One {  
   default void method1() {  
       System.out.println("One-method1");  
   }  
   void method2();  
}  
  
@FunctionalInterface  
interface Two extends One {  
   default void method1() {  
       System.out.println("Two-method1");  
   }  
}  
  
public class TwoImpl implements Two {  
   public void method2() {  
       System.out.println("TwoImpl-method2");  
   }  
  
   public static void main(String[] args) {  
       new TwoImpl().method1();  
   }  
}  
```  

#### RC 6

```java  
  
/*  
    Каким будет результат следующих действий?  
    1/2   1./2   1/2.   1./2.    
 */  
```  

#### RC 7

```java  
/*
Пусть классы Wolf и Rabbit являются наследниками класса Animal. Корректен ли следующий пример?  
*/

public static void main(String[] args) {  
	Wolf w = new Wolf();  
	Animal a = (Animal)w;  
	Rabbit r = (Rabbit)a;  
	if (a instanceof Rabbit) {  
	    Rabbit r = (Rabbit)a;  
	} 
}
  
```  

#### RC 8

```java  
  
// Корректно ли следующие выражение?  
  
public class Test {  
    static void perform() {  
        System.out.println("perform");  
    }  
  
    private Test x;  
    public static void main(String s[]) {  
       x.perform();
    }  
}  
```  

#### RC 9
  
```java  
  
// Выберите все правильные ответы.  
  
private void say(int digit){  
    switch(digit){  
        case 1: System.out.print("ONE ");  
        break;  
        case 2: System.out.print("TWO ");  
        case 3: System.out.print("THREE ");  
        default:System.out.print("Unknown value ");  
    }  
}  

//digit = 0 
//digit = 1 
//digit = 2 
//digit = 3 
```  
  
#### RC 10
  
---  
```java  
/*  
Класс имплементит 2 интерфейса, каждый из которых объявляет одинаковый метод.   
В самом классе - описан метод нужной сигнатуры. Будет ли такое решение работать?  
 */  
public interface IOne {  
	int doAction(String param);  
}  
  
public interface ITwo {  
	int doAction(String param);  
}  
  
public class OneTwo implements IOne, ITwo {  
  
    @Override  
    public int doAction(String param) {  
        return 21;  
    }  
  
    public static void main(String[] args) {  
        var oneTwo = new OneTwo();  
        var result = oneTwo.doAction("param-value");  
        System.out.println(result);  
    }  
}  
```


#### Задача 11

```java  
// Что делает этот код?  
public static <T> Predicate<T> distinctByKey(Function<? super T, ?> keyExtractor) {  
    Map<Object, Boolean> seen = new ConcurrentHashMap<>();  
    return t -> seen.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;  
}  
```

#### Задача 12

```java
//        Что будет выведено в консоль?
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
var unmodifiableList = Collections.unmodifiableList(list);
        list.add(4);
        System.out.println(unmodifiableList);
```

---

```java  
Function<Collection<String>, Map<String, EmployeeInfo>> dataProvider = keys -> {  
    EmployeeFilter employeeFilter = EmployeeFilter.builder().userliames(new ArrayList(keys)).build();  
    List<DcUserDb> users = userService.searchUsers(employeeFilter);  
    List <EmployeeInfo> employeeInfos = users.stream()  
            .filter(user -> predicate == null || predicate.test(user))  
            .map(user -> {  
                EmployeeInfo info = mapper.toOssEmployee(user);  
                ofNullable(handler).ifPresent(consumer -> consumer.accept(info));  
                return info;  
            })  
            .collect(Collectors.tolist());  
    return Employees.builder().employees(employeeInfos).build();  
```  


