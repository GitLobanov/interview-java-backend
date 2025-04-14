Задача 1

```java
public static class A {
    public A() {
        System.out.println("A");
    }
}

public static class B extends A{
    public B() {
        System.out.println("B");
    }
}

public static class C extends B {
    public C() {
        System.out.println("C");
    }
}

public static void main(String[] args) {
    C c = new C();
}
```

1) Что выведет на консоль при вызове метода main(и почему)
2) Что сделать чтобы ничего не выводилось на консоль при вызове метода main
(переопределить конструкторы)

--------------------------------------------------------------------------------
Задача 2

1) Что выведет на консоль при вызове метода main если newMoney окажется меньше нуля
2) Я не знаю какое именно исключение бросит статический метод send класса Notification ,
правильно ли я сделал обьявив catch throwable, если нет то что надо обьявить (какие исключеиня перехватить)
3) Требуется ли аннотация @Transactional над методом drawFromMainAccount
Если требуется то для чего

public class ScratchSecond {

public static void main(String[] args) {

    try{
        new ScratchSecond().drawFromMainAccount(100);
    } catch (Exception e){
        System.out.println("Все плохо");
    }
}

public static class NotEnoughMoneyException extends RuntimeException{}

public void drawFromMainAccount(long amount){
    try{
        long money=amount;
        long newMoney=-100;
        if (newMoney<10){
            try{
                Notification.send("Hello","First");
            } catch (Throwable er){
                System.out.println("тра та та");
            }
        }
        if (newMoney<0){
            throw new NotEnoughMoneyException();
        }
            MainAccount.saveCurrentMoney(newMoney);
        } catch (RuntimeException e){
            System.out.println("Мы здесь");
            System.out.println(e.getMessage());
        }
    }
}

P.S. Было очень много “мутных ”, не вполне однозначных и понятных вопросов в стиле
а что будет если я сделаю то-то или как то там по хитрому изменю код или еще что нибудь

------------------------------------------------------------------------------

Задача 3

-- На входе в банк стоят турникеты - если приложить карточку, то дверки открываются.
-- Сотрудник проходит либо внутрь, либо наружу, куда именно - нигде не записывается
-- Охранник следит, чтобы никто не открывал дверки без прохода, и не проходил без открывания дверок

create table passage (
    card_id bigint not null, -- это ID карточки
    event_time timestamp not null -- момент прохода через турникет
);
create table card (
    id bigserial primary key, -- ID карточки
    employee_fio text not null -- ФИО сотрудника-держателя карточки
);
insert into card values
(1, 'Иванов Иван Иваныч'), -- главный безопасник по стриму Ипотеки
(2, 'Петров Петр'), -- уже неактуальная
(3, 'Сидорова Олеся'), -- уже неактуальная
(4, 'Петров Петр'), -- это Петя потерял карточку, ему сделали новую
(5, 'Сидорова Олеся'), -- Олеся увлолилась, а потом вернулась в банк, но карточку выдали новую
(6, 'Козин Михаил'),
(7, 'Афонин Александр'),
(8, 'Калмыков Сергей'),
(9, 'Легенда Александр'),
(10, 'Остапенко Виктор');

insert into passage values
(1, '2021-01-20 09:00'), -- Иван Иваныч пришел на работу
(4, '2021-01-20 10:00'), -- Петя пришел на работу
(4, '2021-01-20 10:30'), -- Петя пошел за кофе
(4, '2021-01-20 10:40'), -- Петя вернулся с кофе
(4, '2021-01-20 13:45'), -- Петя пошел на обед на улицу
(4, '2021-01-20 15:15'), -- Петя вернулся с обеда
(1, '2021-01-20 18:00'), -- Иван Иваныч пошел домой
(4, '2021-01-20 18:20'), -- Петя пошел домой

-- в 23:00 охранник делает обход, проверяет, что никого в здании нет
-- и до 07:00 следующего рабочего дня офис закрыт, никто в принципе не может войти в здание.

-- В 11:30 21го числа кто-то попытался зайти из сети офиса на GitHub и выложить кусочек кода.
-- Иван Иванович сразу же завел инцидент безопасности. Но кто это мог быть?
-- Для начала стоит проверить, кто физически находился в здании в этот момент...

(1, '2021-01-21 09:00'),
(9, '2021-01-21 09:11'),
(5, '2021-01-21 10:11'),
(6, '2021-01-21 10:13'),
(10, '2021-01-21 10:13'),
(8, '2021-01-21 10:24'),
(4, '2021-01-21 10:43'),
(2, '2021-01-21 10:47'),
(4, '2021-01-21 11:05'),
(5, '2021-01-21 11:12'),
(9, '2021-01-21 11:18'),
(4, '2021-01-21 11:20'),
(10, '2021-01-21 11:28'),
(7, '2021-01-21 11:35'),
(5, '2021-01-21 11:50'),
(9, '2021-01-21 11:57'),
(4, '2021-01-21 12:10'),
(4, '2021-01-21 12:30'),
(10, '2021-01-21 14:38'),
(8, '2021-01-21 14:40'),
(7, '2021-01-21 14:40'),
(6, '2021-01-21 14:40'),
(8, '2021-01-21 15:10'),
(7, '2021-01-21 15:10'),
(6, '2021-01-21 15:11'); -- Ефремов
Написать запрос sql

---------------------------------------------------------------------

SELECT c.employee_fio, COUNT(p_in.event_time)
FROM card c
JOIN passage p_in ON c.id = p_in.card_id
WHERE p_in.event_time <= '2021-01-21 11:30'
GROUP BY c.employee_fio
HAVING COUNT(p_in.event_time) % 2 != 0;

---------------------------------------------------------------------
public class A {

    public Integer i;
    public String str;

}

public List<A> findA(List<A> list){

    return null;
}

1 . Есть класс A с полями как в условии
2. Необходимо реализовать метод findA менять сигнатуру метода запрещено
(нужно найти элемент с А.i=18) , список отсортирован по i

3.Обратить внимание на обработку граничных условий, скорость алгоритма, память.
(подходящее решение – двоичный поиск)


    public List<A> findA(List<A> list) {
        int low = 0;
        int high = list.size() - 1;
        int target = 18;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            A midElement = list.get(mid);

            if (midElement.i == target) {
                return List.of(midElement);
            } else if (midElement.i < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return null;
    }


 List<A> list = List.of(
            new A(10, "a"),
            new A(12, "b"),
            new A(18, "c"),  // Элемент, который ищем
            new A(25, "d"),
            new A(30, "e")
        );

        Solution solution = new Solution();
        List<A> result = solution.findA(list);

        if (result != null) {
            System.out.println("Найден элемент: " + result.get(0).str);
        } else {
            System.out.println("Элемент не найден.");
        }

-----------------------------------------------------------------

Задача 2

!(A&&B);
!(A||B);

Раскрыть скобки и доказать
(Теорема де Моргана)

!(A&&B) = !A||!B;
!(A||B) = !A&&!B;

-----------------------------------------------------------------

Задача 3

Даны две таблицы:

FISH (id int ,name varchar);

CATCH (id catch, fish_id int , quantity int);
Выбрать имена рыб , улов по которым на определенную дату D меньше N; (улов считается меньше N также если он отсутствует вовсе)

-----------------------------------------------------------------


Задача 4

Schema SQL:
1. На входе в банк стоит турникет – если приложить карточку, то дверка открывается...
2. Сотрудник проходит либо внутрь, либо наружу, куда именно - нам не записывается
3. Турникет сделан, чтобы никто из территории двора без прохода, и не проходил без отграничения дверок

create table event (
card_id bigint not null, -- это ID карточки
event_time timestamp not null -- момент прохождения через турникет
);

create table card (
id bigserial primary key, -- ID карточки
employee_fio text not null -- ФИО сотрудника-держателя карточки
);

insert into card values
(1, 'Левченко Иван'), -- главный безопасник по стриму Ипотеки
(2, 'Сидоров Лев'), -- уже неактуальная
(3, 'Коваль Мария'), -- уже неактуальная
(4, 'Петров Петр'), -- это Пётр потерял карточку, ему сделали новую
(5, 'Петров Петр'), -- Олеся уволилась, а потом вернулась в банк, но карточку выдали новую
(6, 'Кузьмин Антон'),
(7, 'Александров Александр'),
(8, 'Лебедев Александр');

insert into passage values
(1, '2021-01-20 08:00'), -- Илья Мальцев пришёл на работу
(5, '2021-01-20 08:00'), -- Петя пришел на работу
(5, '2021-01-20 10:00'), -- Петя пошел за кофе
(5, '2021-01-20 10:03'), -- Петя вернулся с кофе
(5, '2021-01-20 12:00'), -- Петя пошел на обед на улицу
(5, '2021-01-20 12:24'), -- Петя вернулся с обеда
(5, '2021-01-20 18:01'), -- Петя собрал вещи и ушел домой
(5, '2021-01-20 18:12'), -- Петя вернулся домой

-- В 19:00 охрана заметит сбой, проверяет, что никого в здание нет
-- Сотрудникам работающим для офиса закрыт, никто не пройдет и не может войти в здание

-- В 11:30 Игорь пытался зайти на сеть офиса из GitHub и выложить кусочек кода.
-- Иван Немесов сразу же заметил инцидент безопасности. Но кто это мог быть?

insert into passage values
(1, '2021-01-21 08:00'),
(2, '2021-01-21 08:01'),
(3, '2021-01-21 08:03'),
(4, '2021-01-21 08:13');

-----------------------------------------------------------------------------------------
// Заполнить поле children у каждого объекта в списке treeList. Как это сделать?
class Node {
  long id; //заполнен
  Long parentId; //заполнен
  List<Node> children; //пустой
}

class Test {
  public void someMethod(){
    List<Node> treeList = getList();
  }
}

-------------------------------------------------------------------------------

// Будет ли новая транзакция у method2 при вызове method1?
@Service
public class MyServiceImpl{
    @Transactional
    public void method1(){
       //do something
       method2();
    }

    @Transactional(propagation=Propagation.REQUEST_NEW)
    public void method1(){
       //do something
    }
}

-------------------------------------------------------------------------------

create table user (
  id bigserial primary key,
  firstname text,
  lastname text,
  middlename text,
  salary decimal(0,10)
  is_boss Boolean
  group_id digint foreign key on group(id)
);

--- у всех записей user is_boss=false
--- у одной записи user is_boss=true
--- найти всех сотрудников у которых зарплата (salary) больше чем у босса.

create table group (
  id bigserial primary key,
  name text
);

Вопрос к задаче №3. Переходим на приложение Java с Hibernate.
Есть таблица user и что нам необходимо в приложении сделать чтобы с этой таблицей работать?
Вопрос к задаче №3. Теперь добавляется таблица group.
У юзера есть поле group_id которое является внешним ключом на поле id таблицы group. Необходимо связать эти две таблицы.
Вопрос к задаче №3. К примеру мы возьмем таблицу user и
сделаем индекс на поле lastname и потом напишем запрос "select * from user where lastname like %Ivanov" будет ли индекс отрабатывать?


------------------------------------------------------------------------------------------------

   -- Написать запрос выводящий item с наибольшим повторений на каждую дату, т.е. должны получить в ответе на запрос:
-- яблоко 2024-01-01
-- дыня 2024-02-01

CREATE TABLE my_table (
  item VARCHAR(255),
  date_sale DATE
);

INSERT INTO my_table (item, date_sale) VALUES
('яблоко', '2024-01-01'),
('яблоко', '2024-01-01'),
('яблоко', '2024-01-01'),
('апельсин', '2024-01-01'),
('апельсин', '2024-01-01'),
('дыня', '2024-02-01');

WITH SalesCount AS (
    SELECT item, date_sale, COUNT(*) AS sale_count
    FROM my_table
    GROUP BY item, date_sale
),
RankedSales AS (
    SELECT item, date_sale, sale_count,
    ROW_NUMBER() OVER (PARTITION BY date_sale ORDER BY sale_count DESC) AS rn
    FROM SalesCount
)

SELECT    item,    date_sale
FROM RankedSales
WHERE rn = 1;

------------------------------------------------------------------------------------------------

    /**
     * Преобразовать отсортированный массив неуникальных чисел вида [1,1,2,2,3,3,6,7] в массив отсортированных по возрастанию уникальных чисел - [1,2,3,6,7,_,_,_]
     */

class Solution {
    public void removeDuplicates(int[] nums) {
        if(nums == null || nums.length == 0){
            return;
        }

        int count = 1;
        int previous = 0;

        for(int i = 1; i < nums.length; i++){
            if(nums[i] != nums[previous]){
                nums[count++] = nums[i];
                previous = i;
            }
        }

        for(int j = count; j < nums.length; j++){
            nums[j] = 0;
        }

    }
}

[1,1,2,2,3,3,6,7] -> [1,2,3,6,7,_,_,_]

---------------------------------------------------------------------------------------------------
Задача написать класс с тремя методами

    // в метод parse передается строка вида key1=val1;key2=val2;key2=val3... и нужно это все упаковать
    // каким то образом чтобы getRecord возвращала начальную строку
    // getValue сложность O(1)
    // getRecord - кэшируем строку в методе parse в переменную String и возвращаем
    // 1 доп - Сделать так, чтобы могли храниться дубликаты значений (решал через HashMap<String, String> переделал -> HashMap<String, List<String>>)
    // 2 доп сделать класс иммутабельным (метод parse = конструктор и все вспомогательные поля private final геттеры не создавать)

    static class ParseHolder {

        public void parse(String str) {

        }

        public String getRecord() {

        }

        // За O(1)
        public String getValue(String key) {

        }

    }

---------------------------------------------------------------------------------------------

Задача. Можем ли мы набрать элементы на переданную сумму?

([4,2,1], 6) -> true (4 и 2)
([1,8,2], 5) -> false (нету)

boolean twoSum(int[] arr, int targetSum) {

}


-------------------------------------------------------------------------------------------------

```java
import java.io.*;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.Objects;

public class Parser implements Serializable {

    private volatile InputStream is;
    public static List<String> lines;
    private Long linesCount;

    {
        lines = new LinkedList<>();
    }

    public Parser(File file) {
        try {
            is = new FileInputStream(file);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }

    public InputStream getIs() {
        return is;
    }

    public void setIs(InputStream is) {
        this.is = is;
    }

/*    public List<String> getLines() {
        return lines;
    }*/

    public Long getLinesCount() {
        return linesCount;
    }

    public synchronized String read() throws IOException {
        return readFromInputStream(is);
    }

    private String readFromInputStream(InputStream inputStream) throws IOException {
        StringBuffer resultStringBuilder = new StringBuffer();
        linesCount = 0L;
        try (BufferedReader br = new BufferedReader(new InputStreamReader(inputStream))) {
            String line;
            while ((line = br.readLine()) != null) {
                resultStringBuilder.append(line).append("\n");
                lines.addAll(Collections.singleton(line));
                linesCount++;
            }
        }
        return resultStringBuilder.toString();
    }

    public boolean equals(Object o) {
        if (this == o) return true;
        Parser parser = (Parser) o;
        return Objects.equals(is, parser.is) &&
                Objects.equals(lines, parser.lines) &&
                Objects.equals(linesCount, parser.linesCount);
    }
}
```
-----------------------------------------------------------------------------
```java
import java.io.*;
import java.util.*;
import java.security.interfaces.*;

class SignatureService {

    public String signOrder(Object order) {
        //serialize incoming order to string
        String serializedOrder = "{\"order\":\"some test data\"}";

        try {
            RSAPrivateKey rsaPrivateKey = getPrivateKeyFromFile("/path/to/private/key");
            RSAPublicKey publicKey  = getPublicKeyFromFile("/path/to/public/key");
            // some calculations
            return "SIGNED";
        } catch (Exception e) {
            return "NOT_SIGNED";
        }
    }

    private RSAPublicKey getPublicKeyFromFile(String pathToPublicKey) {
        try {
           FileReader reader = new FileReader(pathToPublicKey);
           //read public key
           return publicKey;
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    private RSAPrivateKey getPrivateKeyFromFile(String pathToPrivateKey) {
        try {
            FileReader reader = new FileReader(pathToPrivateKey);
            //read private key
            return privateKey;
        } catch (FileNotFoundException e ) {
            throw new RuntimeException(e);
        }
    }
}
```
--------------------------------------------------------------------------------
Дана строка, состоящая из скобок ({[]}), необходимо проверить строку на валидность.
Валидной строка является та, в которой встречается открывающая и соответствующая ей закрывающая скобка.


--------------------------------------------------------------------------------

```java

public static void main(String[] args) {
    Integer i1 = 128;
    Integer i2 = 128;
    System.out.println("1 - true " + i1.equals(i2));

    Integer i3 = 128;
    Integer i4 = i3; 
    System.out.println("2 - true " + i3.equals(i4));

    Integer i5 = 128;
    Integer i6 = i5; 
    System.out.println("3 - true " + (i5 == i6));

    Integer i7 = 128;
    Integer i8 = i7;
    i7 = 127; //false
    System.out.println("4 - false " + (i7 == i8));

    Integer i9 = 128; 
    System.out.println("5 - true " + i9.equals(128));

    Integer i10 = 129; 
    System.out.println("6 - true " + (i10 == 129));

    Integer i11 = 128; 

    System.out.println("7 - " + (i11.hashCode() == 128.hashCode()));

    Integer i12 = 128;
    Integer i13 = 128; 
    System.out.println("8 - true " + (i12.hashCode() == i13.hashCode()));

    Integer i14 = 128;
    Integer i15 = 128; 
    System.out.println("9 - " + (i14.hashCode().equals(i15.hashCode())));

    Integer i16 = 128;
    Integer i17 = 128; 
    System.out.println("10 - false " + (i16 == i17));

    Integer i18 = 127;
    Integer i19 = 127; 
    System.out.println("11 - true " + (i18 == i19));

    int i20 = 128;
    int i21 = 128; 
    System.out.println("12 - true " + (i20 == i21));

    int i22 = 127;
    int i23 = 127; 
    System.out.println("13 - true " + (i22 == i23));
}

```

-------------------------------------------------------------------------

```java
@Component
public class MyAction {
    public boolean debug = true;
    @Autowired
    public DataSource ds;

    public Collection getCustomers(String firstName, String lastName, String address, String zipCode, String city) throws SQLException {
        Connection conn = ds.getConnection();
        String query = new String("SELECT * FROM customers where 1=1");
        if (firstName != null) {
            query = query + " and first_name = '" + firstName + "'";
        }
        if (firstName != null) {
            query = query + " and last_name = '" + firstName + "'";
        }
        if (firstName != null) {
            query = query + " and address = '" + address + "'";
        }
        if (firstName != null) {
            query = query + " and zip_code = '" + zipCode + "'";
        }
        if (firstName != null) {
            query = query + " and city = '" + city + "'";
        }
        Statement stmt = conn.createStatement();
        ResultSet rs = stmt.executeQuery(query);
        List customers = new ArrayList();
        while (rs.next()) {
            Object[] objects = new Object[] { rs.getString(1), rs.getString(2) };
            if (debug) print(objects, 4);
            customers.add(new Customer(rs.getString(1), rs.getString(2), rs.getString(3), rs.getString(4), rs.getString(5)));
        }
        return customers;
    }

    public void print(Object[] s, int indent) {
        for (int i=0; i<=indent; i++) System.out.print(' ');
        printUpper(s);
    }

    public static void printUpper(Object [] words){
        int i = 0;
        try {
            while (true){
                if (words[i].getClass() == String.class) {
                    String so = (String)words[i];;
                    so = so.toUpperCase();
                    System.out.println(so);
                }
                i++;
            }
        } catch (IndexOutOfBoundsException e) {
            //iteration complete

        }
    }
}
```
----------------------------------------------------------------------

import java.util.*;
import java.util.stream.Collectors;

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

-------------------------------------------------------------------------------------

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.OneToMany;
import java.util.List;
import java.util.Map;
import java.util.Optional;
import java.util.stream.Collectors;

import lombok.Data;


@Entity
@Data
class Student {
  @Id
  private String id;
  private String group;
  @OneToMany
  private List<Book> books;
}

@Entity
@Data
class Book {
  @Id
  private String id;
  private String name;
}

@Repository
interface StudentRepository extends CrudRepository<Student, String> {
  public List<Student> findStudentByGroup(String group);
}

@Service
class StudentService {
  @Autowired
  StudentRepository repository;

  public String mostPopularBook(String group) {

    return repository.findStudentByGroup(group)
      .stream()
      .flatMap(s -> s.getBooks().stream())
      .collect(Collectors.groupingBy(book -> book.getName(), Collectors.counting()))
      .entrySet()
      .stream()
      .max(Map.Entry.comparingByValue())
      .map(Map.Entry::getKey)
      .get();
  }

}

------------------------------------------------------------------------------

Есть список целых чисел, которые встречаются более 2 раз,
Необходимо вывести пары чисел :
<число из списка> –< сколько раз число встречается>
Пример : int mas= [10,15,23,10,15,10,66,10,66,15] -->
10 ->4
15 ->3

-------------------------------------------------------------------------------

public static void counter()!
    int count = 0;
    String s1 = "java";
    String s2 = "java";
    StringBuilder s3 = new StringBuilder("java");
    s2. toUpperCase();
    if (s1 == s2) count++;
    if (s1.equals(s2)) count++;
    if (s1 == s3) count++;
    if (s1.equals(s3)) count++;
    System. out. println(count);
}

-----------------------------------------------------------------------------
abstract class Camel
    void travel();
}

interface EatsGrass (
    protected int chew();
}

abstract class Elephant {
    private abstract class SleepsALot (
        abstract int sleep();
    }
}

class Eagle (
    abstract fly();
}

----------------------------------------------------------------

--------------------------------------------------------------------------------
public abstract class Test implements Runnable {

    private Object lock = new Object();

    public void lock() {
        synchronized (lock) {
        try {
            lock.wait();
            System.out.println("1");
        } catch (InterruptedException e) {
        }
    }
}


public void unlock() {
    synchronized (lock) {
        lock.notify();
        System.out.println("2");
    }
}

public static void main(String s[]) {

    new Thread(new Test() {
        public void run() {
            lock();
        }
    }).start();
    new Thread(new Test() {
        public void run() {
            unlock();
        }
    }).start();
    }
}

--------------------------------------------------------------------------------
public static void main(String[] args) {
    List<StringBuilder> list = Arrays.asList(new StringBuilder("Java"), new StringBuilder("Hello"));
    list.stream().map((x) -> x.append("World"));
    list.forEach(System.out::print);
}

--------------------------------------------------------------------------------

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

    public static class TwoImpl implements Two {
        public void method2() {
            System.out.println("TwoImpl-method2");
        }

        public static void main(String[] args) {
            TwoImpl t = new TwoImpl();
            t.method1();
        }
    }
```

---------------------------------------------------------------------

```java
// Корректен ли следующий код?
public class Test {
    private int id;
    public Test(int i) {
        id=i;
    }
    public static boolean test(Test t, int id) {
        return t.id==id;
    }
}

// getter for id
// not null checked
```

----------------------------------------------------------------------------------

Каким будет результат следующих действий?
1/2   1./2   1/2.   1./2.

// 0, 0.5, 0.5, 0.5

----------------------------------------------------------------------------------

Пусть классы Wolf и Rabbit являются наследниками класса Animal. Корректен ли следующий пример?
Wolf w = new Wolf();
Animal a = (Animal)w;
Rabbit r = (Rabbit)a;

/*
вызовет ClassCastException.
if (a instanceof Rabbit) {
    Rabbit r = (Rabbit)a;
}
*/

---------------------------------------------------------------

Корректно ли следующие выражение?
public class Test {
    static void perform() {
    	System.out.println("perform");
    }

    private Test x;
    public static void main(String s[]) {
       x.perform(); // корректно ли это выражение?
    }
}

----------------------------------------------------------------------------

Выберите все правильные ответы.

private void say(int digit){
    switch(digit){
        case 1: System.out.print("ONE ");
        break;
        case 2: System.out.print("TWO ");
        case 3: System.out.print("THREE ");
        default:System.out.print("Unknown value ");
    }
}

digit = 1 ONE
digit = 0 TWO TREE
digit = 2 TWO Unknown value
digit = 3 THREE Unknown value


```java
// Что произойдет при попытке выполнения данного кода:

public class Overload {

    public void method(Object o) {
        System.out.println("Object");
    }

    public void method(java.io.FileNotFoundException f) {
        System.out.println("FileNotFoundException");
    }

    public void method(java.io.IOException i) {
        System.out.println("IOException");
    }

    public static void main(String args[]) {
        Overload test = new Overload();
        test.method(null);
    }
}

//Ошибка компиляции
//Ошибка времени выполнения
//«Object»
//«FileNotFoundException»
//«IOException»

```

```java
Что произойдет при попытке выполнения данного кода:

public static void main(String[] args) {
    List<StringBuilder> list = Arrays.asList(
        new StringBuilder("Java "),  new StringBuilder("Hello "));
    list.stream().map((x) -> x.append("World  "));
    list.forEach(System.out::print);
}

//Ошибка компиляции
//На консоли появится «Java Hello »
//На консоли появится «Java World Hello World »
//Ошибка времени исполнения

```
-----------------------------------------------------------------------

```java

public class ConfirmDocService {
    @Autowired
    ConfirmDocRepository confirmDocRepository;
    @Autowired
    ConfirmDocStateMachineService confirmDocStateMachineService;
    @Autowired
    ConfirmDocMapper confirmDocMapper;
    @Autowired
    StateMachineFactory<DocStatusState, DocStatusEvent> confirmDocStateMachineFactory;

    @Transactional
    public ConfirmDocDto createConfirmDoc (ConfirmDocDto confirmDocDto) {
        ConfirmDoc confirmDoc = confirmDocMapper. toEntity(confirmDocDto);
        StateMachine<DocStatusState, DocStatusEvent> sm = confirmDocStateMachineFactory.getStateMachine();
        sm.getExtendedState().getVariables().put("CONFIRM_DOC_TO_BE_SAVED", confirmDoc);
        sm.start ();
        confirmDoc. setDocStatusId(sm.getState ().getId());
        ConfirmDoc savedConfirmDoc = saveOrUpdate (confirmDoc);
        return confirmDocMapper.toDto (savedConfirmDoc);
    }
    @Transactional
    public ConfirmDoc saveOrUpdate (ConfirmDoc confirmDoc) {
        if (confirmDocRepository.findAll().stream().filter(it -> it.id = confirmDoc.Id).count () == 1) {
            ConfirmDoc confirmDocFromDb = confirmDocRepository. findById (confirmDoc. Id);
            ConfirmDoc updConfirmDoc = confirmDocMapper. update (confirmDocFromDb, confirmDoc);
            return confirmDocRepository. save (confirmDoc);
        } else {
            return confirmDocRepository. save (confirmDoc);
        }
    }
}
```

----------------------------------------------------

```java

public static void main(String[] args) {
    try {
        try {
            throw new Exception("");
        } catch (RuntimeException e) {
            System.out.print("1");
        } finally {
            System.out.print("2");
            System.out.print("3");
        }
    } catch (Exception e) {
        System.out.print("4");
    } finally {
        System.out.print("5");
        System.out.print("6");
    }
}
```
---------------------------------------------------
@Entity
@Table(name="triangle", schema="application")
class Triangle (
    @Id
    private UUID id;
    @Column
    private int a;
    @Column
    private int b;
    @Column
    private int c;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass()
        != o.getClass()) return false;
        Triangle triangle = (Triangle) 0;
        return a == triangle. a && b == triangle. b && C= triangle. c;
    }

    @Override
        public int hashCode()
        return com. google. common. base. Objects. hashCode(id, a, b, c);
    }
}

-------------------------------------------------------------------

```java

public class MyObjectEquals {

    public static void main (String[] args) {
        MyObject obj = new MyObject(10);
        HashSet<MyObject> set = new HashSet<>();
        set.add(obj);
        obj.set(1000);

        System. out. println(set.contains(obj));
        System. out. println(set.contains(new MyObject(10)));
        System. out. println(set.contains(new MyObject(1000)));
        System. out. println(Objects.equals(new MyObject(1000), obj));
        System. out. println(obj.equals(new MyObject(1000)));
    }
    static class MyObject {
        private int i;
        public MyObject(int i) {
            set(i);
        }
        public void set(int i) {
            this.i = i;
        }

        public int hashCode() {
            return i;
        }

        @Override
        public boolean equals(Object object) {
            if (this == object) return true;
            if (object == null || getClass() != object.getClass()) return false;
            MyObject myObject = (MyObject) object;
            return i == myObject.i;
        }
    }
}
```

----------------------------------------------------------

```java

@Service
public class AccountService {
    private final AccountRepository repository;
    private final NotificationRestService notificationRest;

    public AccountService(AccountRepository repository, NotificationRestService notificationRest) {
        this. repository = repository;
        this. notificationRest = notificationRest;
    }

    public void registerAccount(Account acc) {
        createAccount(acc);
        notificationRest.notify(acc);
    }

    @Transactional
    public void createAccount(Account acc) {
        repository. createPersonalAccount(acc);
    }
}
```
------------------------------------------------------------------
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
------------------------------------------------------------------

```java

// Провести код ревью

//Компонент проведения платежей
// Платеж — это сумма в валюте, которая переводится от одного клиента к другому
//Сумма платежа при сохранении должна быть пересчитана в рубли по курсу ЦБ на текущую дату
//При платеже должна быть выставлена комиссия, которая рассчитывается в зависимости от суммы платежа
//После платежа надо вызвать сервис нотификаций,который прокинет нотификации пользователям — для клиентов будет выглядеть как push-уведомление
//Компонент переводит деньги от залогиненного пользователя переданному на вход


@Service
public class MoneyTransferService {

    @Autowired
    private TransferRepository transferRepository;

    @Autowired
    private ComissionRepository commisionRepository;

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private NotificationService notificationService;

    @Autowired
    private ExchangeRateRestClient exchangeRateRestClient;

    @Transactional
    public void processTransfer(double amount, Currency currency, Long recepientId) {
        double amountInRecepientCurrency = amount * exchangeRateRestClient.fetchExchangeRate().getRateForCurrency(currency.getCode());
        Long userId = SecurityContextHolder.getContext().getAuthentication().getPrincipal().getId();
        User user = userRepository.findUserById(userId);
        Transfer transfer = new Transfer(amountInRecepientCurrency, user, recipientId);
        transferRepository.save(transfer)
        if (amountInRecepientCurrency < 1000) {
            Commission commission = new Commission(amountInRecepientCurrency * 0.015, user);
            commissionRepository.save(commission);
        }
        if (amountInRecepientCurrency > 1000) {
            Commission commission = new Commission(amountInRecepientCurrency * 0.01, user);
            commissionRepository.save(commission);
        }
        if (amountInRecepientCurrency > 5000) {
            Commission commission = new Commission(amountInRecepientCurrency * 0.005, user);
            commissionRepository.save(commission);
        }
        try {
            notificationService.sendNotification(transfer);
        } catch (Exception e) {
            // ignore the exception and continue;
        }
    }
}
```

-------------------------------------------------------------

```java

///Что будет выведено на экран?

public class MeaningOfLife{

    private final ExecutorService executor = Executors.newFixedThreadPool(10);
    
    Future <Integer> findElseWhere(Integer currentAnswer){
        return executor.submit(() -> {
            Thread.sleep(1000L);
            return currentAnswer + 6;
        });
    }

    CompletebleFuture exploreDifferentMeaning (Integer currentAnswer) {
        return CompletebleFuture.supplyAsync(() -> {
            try{
                Thread.sleep(2000L);
            }catch(InterruptedException e){
                throw new RuntimeException(e);
            }
            return currentAnswer + 10;
        });
    }

    static void announceAnswer(Integer answer){ 
        System.out.println(answer);
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException{
        Integer answer = 42;
        MeaningOfLife meaningOfLife = new MeaningOfLife();
        Future<Integer> elseWhereAnswer = meaningOfLife.findElsewhere(answer);
        meaningOfLife.exploreDifferentMeaning(answer).thenAccept(MeaningOfLife::announceAnswer);
        announceAnswer(elsewhereAnswer.get());
        announceAnswer(elsewereAnswer.get());
        announceAnswer(answer);
    }
}
```

-------------------------------------------------------

```java
@Service
public class ServiceTesting {

    private final EqualsCarList equalsCarList;

    public ServiceTesting(EqualsCarList equalsCarList) {
        this.equalsCarList = equalsCarList;
    }

    public String getName(Boolean flag){
        if(flag){
            return "hello";
        }
            return "world";
        }
    }

    public String getNameWithMethod(Boolean flag){
        if(flag){
            return equalsCarList.getStringMethod();
        }
            return "world";
        }
    }

    public String getNameWithException(Boolean flag,Integer age){
        if(flag & age > 22){
            return "hello";
        }else if(flag & age<22){
            throw new IllegalArgumentException("exception age");
        }
            return "world";
        }
    }

    @Componet
    public class EqualsCarList {

        public String getStringMethod() {
            return "hello";
        }
    }
}
```


---------------------------------------------------------
Реализовать дабл чек синглотоне

public class Singletone() {

    private static volatile Connection connection;

    public synchronized static Connection getConnection (){
        if (connection == null) {
            synchronized (Singletone.class){
                if (connection == null) {
                    connection = new Connection();
                }
            }
        }
        return connection;
    }

}
-----------------------------------------------------------------

```java
// Предложите эффективный алгоритм удаления нескольких рядом стоящих элементов из середины списка, реализуемого ArrayList.

// 1111111258 874 123   874   11111112584123

public static void main(String[] args) {
    var list = List.of(1111111258 874 123);
    var sublist = List.of(8, 7, 4);

    int startSublistPosition = 0;
    int startPosition = -1;

    for (int i = 0; i < list.size; i++) {
        list.get(i) == sub

        list.set(i) = list.set(i + sublist.size())

        // либо обнуляем, либо оставляем
    }
}

```
-----------------------------------------------------------------
```sql
// employee(id, department_id, name, salary)
// department(id, name)
// вывести список сотрудников (id, name), имеющих макс зп в своем отделе

SELECT id, name
FROM employee
WHERE salary in (
    SELECT MAX(salary)
    FROM employee
    GROUP BY department_id
)
```

-----------------------------------------------------------------

```sql
table1
-----
col1
-----
a
a
b
c
null
*************************

table2
-----
col2
-----
a
b
b
null
null

// a a b b
```
-----------------------------------------------------------------