Задача 1
Какой результат выведет вызов метода foo()?

public void foo() {
    String m "Hello";
    System.out.print(m);
    bar(m);
    System.out.print(m);
}
public void bar(String m) {
    m+World!";
}

--------------------------------------------------------------------------------------
---- Задача 2  ----
{
    String a = "10";
    String b = new String("10");
    String c "10";
    System.out.println(a = b); //
    System.out.println(a ==c); //
}
Задача 3

try {
    throw Exception1;
} finally {
    throw Exception2;
}

--------------------------------------------------------------------------------------
---- Задача 4 ----
Что выведется в консоль

public class Person {
    private final long value = 10;

    public void doSomething(){
        System.out.println("Person does something...");
    }
    public long getValue() {
        return value;
    }
}
public class Student extends Person {
    private final long value = 8;

    @Override
    public void doSomething() {
        System.out.println("Student does something...");
    }
    public void printvalue(){
        System.out.println(getValue());
    }
    @Override
    public long getValue() {
        return value;
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Student();
        person.doSomething();
        person.printValue();
    }
}

--------------------------------------------------------------------------------------
---- Задача 5 ----
Каким будет результат выполнения данного кода?
public static void main(String[] args) {
    List<String> stringList = new ArrayList<>();
    stringList.add("one");
    stringList.add("one and a half");
    stringList.add("two");
    stringList.add("two and a half");
    stringList.add("three and a half");
    System.out.println("Before" + stringList);
    Iterator<String> stringIterator = stringList.iterator();

    while (stringIterator.hasNext()) {
        String next stringIterator.next();
        if (next.equals("two and a half")) {
            stringList.add("three");
        }
    }
    System.out.println("After " + stringList);
}
// CopyOnWriteArrayList

--------------------------------------------------------------------------------------
Задача 6
@Data
class Phone{
    private String name;
    private int price;

    public Phone(String name, int price) {
        this.name = name;
        this.price = price;
    }
}

Вывести в консоль все телефоны, цена которых менее 50000;
public class Main {
    public static void main(String[] args) {
        Stream<Phone> phoneStream = stream.of(new Phone ("iPhone 6 S", 54000),
            new Phone("Lumia 950", 45000),
            new Phone("Samsung Galaxy S 6", 40000));
        //
    }
}

--------------------------------------------------------------------------------------
Задача 7
@Data
public class Beneficiary {
    private String name;
    private List<Account> accounts;
    public Beneficiary (String name, List<Account> accounts) {
        this.name = name;
        this.accounts = accounts;
    }
}

@Data
public class Account {
    private String number;
    private long balance;
    public Account(String number, long balance) {
        this.number = number;
        this.balance = balance;
    }
}

С помощью stream API вывести в конcоль номера аккаунтов, баланс которых больше или равен 10000.
public class Main {
    public static void main(String[] args) {
    Beneficiary beneficiary1 = new Beneficiary("Oleg", generate Accounts());
    Beneficiary beneficiary2 = new Beneficiary ("Kir", generateAccounts());
    Beneficiary beneficiary3 = new Beneficiary("Yulya", generateAccounts());
    List<Beneficiary> beneficiaries = List.of (beneficiary1, beneficiary2, beneficiary3);
    //
}

--------------------------------------------------------------------------------------
Задача 8

public class Main {
public static void main(String[] args) {

    String[][] inputData = new String[] [] {
        {"10001", "John", "true"),
        {"10005", "Mike", "true"),
        ("10009", "Don", "false"}
    };
    List<Account> closedAccounts = getclosedAccounts (List.of(inputData));
        System.out.println(closed Accounts);
    }
    private static List<Account> getclosed Accounts (List<String[]> accounts) {
        //TODO...
    }

    class Account {
        String number;
        String name;
        boolean closed;
        public Account() {
        }
        public Account(String number, string name, boolean closed) {
            this.number = number;
            this.name = name;
            this.closed = closed;
        }
        //getters and setters
    }
}

--------------------------------------------------------------------------------------
Задача 9
Дан массив int[] array (3, 4, 5, 1, 3, 5, 7, 9, 9, 0, 5, 3};
Реализовать метод, который будет возвращать подсчитанное количество цифр.
Пример:
3 3
4 1
5 3
и т.д.

--------------------------------------------------------------------------------------
Задача 10
Привести сущность к требованиям.

@Entity
public final class User {
    private final String name;

    @OneToMany (mapped By = "user")
    private Set<Phone> phones = new HashSet<>();

    @OneToMany (mappedBy = "user")
    private final Set<Address> addresses = new HashSet<>();

    public User(String name) {
        this.name = name;
    }
}

--------------------------------------------------------------------------------------
Задача 11

public class Service {
    @Transactional
    public void a() {
        b();
    }
    @Transactional (propagation = Propagation. REQUIRES_NEW)
    public void b() {
        c();
    }
    @Transactional (propagation = Propagation. REQUIRES_NEW)
    void c() {
        // something..
    }
}

--------------------------------------------------------------------------------------
Задача 12
// Инвертировать входящий массив
// {1, 2, 3, 4, 5}

public int[] array Reverse (int[] array) {
    //
}