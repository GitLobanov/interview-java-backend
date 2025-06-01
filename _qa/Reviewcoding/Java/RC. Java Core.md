#### Задача 1

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
  
#### Задача 2
  
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

#### Задача 3
  
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

#### Задача 4
  
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
  
#### Задача 5
  
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

#### Задача 6

```java  
  
/*  
    Каким будет результат следующих действий?  
    1/2   1./2   1/2.   1./2.    
 */  
```  

#### Задача 7

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

#### Задача 8

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

#### Задача 9
  
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
  
#### Задача 10
  

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

#### Задача 13

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

#### Задача 14

```java
static class Parent {  
  
    public int age = 50;  
  
    public void method() {  
        System.out.println("parent method work");  
    }  
    public void printAge() {  
        System.out.println(age);  
    }
}
  
static class Child extends Parent {  
    public int age = 20;  
  
    @Override  
    public void method() {  
        System.out.println("child method work");  
    }
}
public static void main(String[] args) {  
    Parent object = new Child();  
    object.method();  
    object.printAge();  
}
```

#### Задача 15

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] arr = new int[5];
            arr[100] = 500;
            System.out.println(1);
        } catch (ArithmeticException e) {
            System.out.println(2);
        }
    }
}
```

#### Задача 16

```java
public class Incrementer {
    static int base = 10;
    public static int increment() {
        return ++base;
    }
    public static void main(String[] args) {
        System.out.println(Incrementer.increment());
        System.out.println(Incrementer.increment());
    }
}
```

#### Задача 17

```java
System.out.print("Hello");
while (1) {
    System.out.println(" World");
    break;
}
```

#### Задача 18

```java
int day = 4;
switch (day) {
    case 1:
        System.out.print("One");
        break;
    case 2:
    case 4:
        System.out.print("Four");
        day = 1;
    case 5:
        System.out.print("Five");
}
```

#### Задача 19

```java
public class Main {
    public static void main(String[] args) {
        var set = new HashSet<Integer>();
        Random random = new Random();
        for (int i = 0; i < 100; i++) {
            set.add(random.nextInt(10));
        }
        System.out.println(set.size());
    }
}
```

#### Задача 20

```java
interface A { default void method() { System.out.println("A"); } }
interface B { default void method() { System.out.println("B"); } }
class C implements A, B {} 
```

Решение:

Несмотря на `default`-реализации, если класс реализует два интерфейса с **одинаковыми методами по сигнатуре**, он **обязан** явно переопределить конфликтующий метод, даже если оба интерфейса предоставляют реализацию. Будет ошибка компиляции
#### Задача 21

Включен Parallel GC, была обнаружена утечка памяти, коллега указал что здесь проблема в циклическое зависимости, так ли это?

```java
class Node {
    private Node edge;
    public void setEdge(Node edge) { this.edge = edge; }
}

public void createAndDelete() {
	// здесь циклическая зависемость?
    Node node1 = new Node();
    Node node2 = new Node();
    node1.setEdge(node2);
    node2.setEdge(node1);
}
```

#### Задача 22

```java
public class Main {
    static boolean foo(char a) {
        System.out.println(a);
        return true;
    }

    public static void main(String[] args) {
        int i = 0;
        for (foo('A'); foo('B') && (i < 2); foo('C')) {
            i++;
            foo('D');
        }
        foo('D');
    }
}
```

#### Задача 23

```java
public class Main {
    public static void foo(Integer i) {
        System.out.println("foo(Integer)");
    }
    public static void foo(short i) {
        System.out.println("foo(short)");
    }
    public static void foo(long i) {
        System.out.println("foo(long)");
    }
    public static void foo(Object i) {
        System.out.println("foo(object)");
    }
    public static void foo(int... i) {
        System.out.println("foo(int...)");
    }
    public static void main (String[] args) {
	    foo (10);
    }
}
```

#### Задача 24

```java
// Какой вариант инициализации переменной приведет к ошибке?
class Box<T> {
    T value;
    public Box (T value) {
        this.value = value;
    }
}

public static void main(String[] args) {
    Box<? super String> b1 = new Box<> (123);
    Box<? extends String> b2 = new Box<>("123");
    Box<? extends Number> b3 = new Box<>(123);
    Box<? super Number> b4 = new Box<>(123L);
}
```

#### Задача 25

```java
// Что выведеться?
class Test {
    static void m(Object o) {
        System.out.println("one");
    }

    static void m(String[] o) {
        System.out.println("two");
    }

    static <T> T g() {
        return null;
    }

    public static void main(String[] args) {
        m(g());
    }
}
```

#### Задача 26

Классы иммутабельные?

```java
public class ImmutableBankTransaction {
    
    public StringBuilder transactionId;
    public StringBuilder fromAccount;
    public StringBuilder toAccount;
    public BigDecimal amount;
    public LocalDateTime timestamp;
    public Map<String, StringBuilder> metadata;
    public List<String> statusHistory;

    public ImmutableBankTransaction(
            StringBuilder transactionId,
            StringBuilder fromAccount,
            StringBuilder toAccount,
            BigDecimal amount,
            LocalDateTime timestamp,
            Map<String, String> metadata,
            List<String> statusHistory) {
        this.transactionId = transactionId;
        this.fromAccount = fromAccount;
        this.toAccount = toAccount;
        this.amount = amount;
        this.timestamp = timestamp;
        this.metadata = metadata;
        this.statusHistory = statusHistory;
    }
}

public class ImmutableBankTransactions {
    
    public HashMap<StringBuilder, ImmutableBankTransaction> immutableHashMap = HashMap();

    public ImmutableBankTransactions(HashMap<StringBuilder, ImmutableBankTransaction> outsideMap) {
	    immutableHashMap = outsideMap;
    }

	public ImmutableBankTransaction getByTransactionId(StringBuilder transactionId) {
		return immutableHashMap.get(transactionId);
	}
}
```