#### Задача 1

```java  
import org.springframework.transaction.annotation.Transactional;  
  
public class AccountService {  
  
    public static void main(String[] args) {  
        try {  
            new AccountService().drawFromMainAccount(100);  
        } catch (Exception e) {  
            System.out.println("Все плохо");  
        }  
    }  
  
    public static class NotEnoughMoneyException extends RuntimeException {  
    }  
  
    @Transactional  
    public void drawFromMainAccount(long amount) {  
        try {  
            long newMoney = MainAccount.currentMoney - amount;
            if (newMoney < 10) {  
                try {  
                    NotificationService.send("Hello", "First");  
                } catch (Throwable er) {  
                    System.out.println("тра та та");  
                }  
            }  
            if (newMoney < 0) {  
                throw new NotEnoughMoneyException();  
            }  
            MainAccount.saveCurrentMoney(newMoney);  
        } catch (RuntimeException e) {  
            System.out.println("Мы здесь");  
            System.out.println(e.getMessage());  
        }  
    }  
}  
```  
  
#### Задача 2
  
```java  
/**  
 * Что здесь не так?  
 */  
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
```  
  
#### Задача 3
  
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
  
    public List<String> getLines() {  
        return lines;  
    }  
  
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
  
/*  
- Нет serialVersionUID  
- InputStream не сериализуем (должен быть transient)  
- getLines() { Collections.unmodifiableList(lines);  
- equals          
 */  
```  
  
  
#### Задача 4
  
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
  
#### Задача 5
  
```java  
/*  
Реализация фильтра по пользователям, что не так?          
 */
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
#### Задача 6
  
```java  
  
/*  
    Все в коде хорошо?  
 */  
  
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
```  

#### Задача 7

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

#### Задача 8

```java  
/*  
        Все ли хорошо?  
 */  
@Entity  
@Table(name="triangle", schema="application")  
class Triangle {  
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
        return a == triangle.a && b == triangle.b && c = triangle.c;  
    }  
  
    @Override  
    public int hashCode() {  
        return com.google.common.base.Objects.hashCode(id, a, b, c);  
    }  
}  
  
/*  
        return Objects.hash(id, a, b, c);  
        return Objects.hash(a, b, c);  // Без id  
 */  
```  

#### Задача 9

```java  
/*  
        Что не так?  
 */  
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
        repository.createPersonalAccount(acc);  
    }  
}  
```  

#### Задача 10

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

#### Задача 11

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

#### Задача 12

```java  
@RestController  
@RequestMapping("/api/user")  
public class UserController {  
  
    @Autowired  
    private UserService userService;  
  
    private final ExecutorService executorService = Executors.newFixedThreadPool(5);  
    private volatile int counter;  
  
    @GetMapping("/{id}")  
    public User getUser(@PathVariable String id) {  
        executorService.submit(() -> {  
            try {  
                counter++;  
                User userData = userService.getUserData(id);  
                System.out.println("User Data retrieved: " + userData);//  
            } catch (Exception e) {  
                System.err.println("Error fetching user data.");//  
            }  
        });  
        return userData;  
    }  
}  
```  

#### Задача 13
  
```java  
@Service  
@RequiredArgsConstructor  
public class UserService {  
  
    private final UserRepository userRepository = new UserRepository();  
  
    private final Map<String, User> cache = new HashMap<>();  
  
    @Cached  
    public Optional<User> getUserData(String userId) {  
        if (cache.containsKey(userId)) {  
            return cache.get(userId);  
        }  
        Optional<User> user = userRepository.findById(userId);  
        cache.put(userId, user);  
        return user;  
    }  
}  
```  

#### Задача 14
  
--------------------------------------------------------------  
  
```java  
  
/**  
API поиска авторов и их книг по имени автора  
Также компонент при каждом поиске обновляет статистику по частоте использования поисковой строки(сбрасывается раз в сутки другой системой)  
При обнаружении популярного запроса (> 1000 запросов в сутки), по которому находится много авторов, отправляется алерт  
Алерт должен отправляться не более 1 раза за сутки для каждого автора  
Все классы на самом деле находятся в разных файлах, однако здесь представлены в одном месте для удобства  
*/  
@RestController  
public class AuthorController {  
  
    @Autowired  
    private AuthorSearchService service;  
  
    @GetMapping("/authors")  
    public List<AuthorDto> readAllAuthors(@RequestParam String query) {  
        List<Author> authors = service.search(query);  
        return authors.stream().map(el -> {  
            return new Mapper().map(el);  
        }).collect(Collectors.toList());  
    }  
}  
  
  
@Component  
public class AuthorSearchService {  
  
    @Autowired  
    private AuthorRepository authorRepository;  
  
    @Autowired  
    private StatisticsRepository statisticsRepository;  
  
    private AlertRestClient arc = new AlertRestClient;  
  
    @Transactional  
    public List<Author> search(String query) {  
        List<Author> author = authorRepository.findByNameContainingIgnoreCase(query);  
        Statistics s = statisticsRepository.findById(query).orElse(null);  
        if(s==null) s = new Statistics(query);  
        s.setNumbers(s.getNumbers() + 1);  
        statisticsRepository.save(s);  
  
        if(s.getNumbers() > 1000 && authors.size() > 1000) {  
            System.out.println("too popular with too much data, sending an alert...");  
            arc.send(query, s.getNumbers(), authors.size());  
        }  
        return author;  
    }  
}  
  
  
public interface AuthorsRepository extends CrudRepository<Author, Long> {  
    List<Author> findByNameContainingIgnoreCase(String name);  
}  
  
  
@Data  
@Entity  
public class Author {  
    @Id  
    @GeneratedValue    private Long id;  
  
    private String name;  
  
    @OneToMany(mappedBy = "author")  
    private List<Book> books;  
  
    public Author(String name){  
        this.name = name;  
    }  
}  
  
  
@Data  
@Entity  
public class Statistics {  
    @Id  
    private String query;  
  
    private int numbers;  
  
    public Statistics(String query){  
        this.query = query;  
    }  
}  
  
  
@Data  
public class AuthorDto {  
    private Long id;  
    private String name;  
    private List<Book> books;  
}  
  
@Data  
@Entity  
public class Book {  
    private Long id;  
    private String name;  
}  
```  

#### Задача 15

```java  
/*  
Что не так с кодом?  
Как исправить lazyinitializationexception?  
А где тут N+1?  
Как можно решать N+1?  
Как решить на HQL?  
В чем разница Spring vs Spring Boot?  
Какие паттерны использует Spring?  
Можно ли в рамках одного приложения обьявить два Singleton?  
- Dead letter que  
- Или складывать операции в outbox и проходить шедулером  
*/  
  
@Getter  
@Setter  
@Entity  
@Table(name = "route", schema = "rm")  
public class Route {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Integer id;  
    @ManyToMany(fetch = FetchType.LAZY)  
    @JoinTable(schema = "rm", name = "route_x_kafka_address",  
    joinColumns = @JoinColumn(name = "route_id", referencedColumnName = "id"),  
        inverseJoinColumns = @JoinColumn(name = "kafka_address_id", referencedColumnName = "id")  
    )  
    private Set<KafkaAddress> kafkaAddresses;  
  
}  
  
@Service  
@RequiredArgsConstructor  
public class RouteService {  
  
    private final RouteRepository repository;  
  
    public boolean isRouteHasEmptyKafkaAddress(int routeId) {  
        Route route = repository.findById(routeId)  
            .orElseThrow(() -> new NotFoundException("Route not found"));  
        Set<KafkaAddress> kafkaAddresses = route.getKafkaAddresses();  
  
        return kafkaAddresses.isEmpty();  
  
    }  
  
}  
```  
  
#### Задача 16
  
```java  
  
Провести ревью кода:  
  
/**  
 *  Задача - начислить всем работникам, которые провели за последний период больше 50-ти интервью бонус в 15% от ЗП,  
 *  а тем, кто провел больше 100 - 30%. И отправить каждому сообщение.  
 */  
@Component  
public class EmployeeService {  
  
    @Autowired  
    private final EmployeeRepository repository;  
    @Autowired  
    private final PaymentService paymentService;  
    private final NotificationClient client = new NotificationClient();  
  
    @Transactional  
    public void updateAll() throws NotificationClientIOException {  
        try {  
            repository.findAll().forEach(emp -> update(emp.getId()));  
        } catch (Throwable t) {  
            //do nothing  
        }  
    }  
  
    @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)  
    private void update(Long id) throws NotificationClientIOException {  
        Employee employee = repository.findById(id).get();  
        if (employee.interviewsLastPeriod > 50) {  
            double bonus = 0.15 * employee.salary.getAmount();  
            paymentService.processPayment(bonus, id);  
            client.send(id);  
        }  
        if (employee.interviewsLastPeriod > 100) {  
            double bonus = 0.3 * employee.salary.getAmount();  
            paymentService.processPayment(bonus, id);  
            client.send(id);  
        }  
        System.out.println("Finished transaction for employee");  
    }  
  
    @Data  
    @Entity    class Employee {  
  
        private final int interviewsLastPeriod;  
        @ManyToOne  
        private final Salary salary;  
  
        public Employee(int interviewsLastPeriod, Salary salary) {  
            this.interviewsLastPeriod = interviewsLastPeriod;  
            this.salary = salary;  
        }  
    }  
}  
  
@Component  
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)  
public class PaymentService {  
    //...//  
    public void processPayment(double amount, Long id) {  
        //...//  
    }  
}  
```  

#### Задача 17

```java  
//Есть класс  
@Data  
@Builder  
@AllArgsConstructor  
public class Message {  
    private String code;  
    private String message;  
    private String format;  
}  
  
// Есть конфигуратор  
@Data  
@Configuration  
@ConfigurationProperties(prefix= "messages")  
public class MessageConfig {  
    /**  
    Тексты сообщений с кодами.  
    **/  
    private Map<String, Message> lines;  
}  
/*  
Пример YAML файла  
messages:  
    lines:  
        VALIDATION_ERROR:  
            code: 800  
            message: Не пройден форматно-логический контроль.  
            format: "Поле Xs: Xs.  
        3SON_FORMAT_ERROR:  
            code: 800  
            message: Некорректный формат входного json.  
*/  
```  
  
#### Задача 18
  
```java  
/**  
* Account.  
*  
* @author Author  
  */  
  @Data  
  @Entity  public class Account {  
      @Id  
      private Long id;  
      private String accountNumber;  
      private String bic;  
      private Double amount;  
  }  
  
/**  
* AccountRepository.  
*  
* @author Author  
  */  
  public interface AccountRepository extends JpaRepository<Account, Long> {  
      
      List<Account> findByAccountNumber(String accountNumber);  
  }  
  
/**  
* BalanceService.  
*  
* @author Author  
  */  
  @Service  
  public class BalanceService {  
  
  @Autowired  
  AccountRepository accountRepository;  
  
    public Account change(Long id, Double diff) {  
      Account account = accountRepository.getOne(id);  
      account.setAmount(account.getAmount() + diff);  
      return accountRepository.save(account);  
    }  
  }  
  
/**  
* AccountResource.  
*  
* @author Author  
  */  
  @RestController  
  @RequiredArgsConstructor  public class AccountResource {  
  
  private final AccountRepository accountRepository;  
  private final BalanceService balanceService;  
  
  @GetMapping(value = "/accounts/add",  
  produces = MediaType.APPLICATION_JSON_VALUE)  
  Account add(Account account) {  
    return accountRepository.save(account);  
  }  
  
  @GetMapping(value = "/accounts/{accountNumber}",  
  produces = MediaType.APPLICATION_JSON_VALUE)  
  List<Account> findByAccountNumber(@PathVariable("accountNumber") String accountNumber) {  
    return accountRepository.findByAccountNumber(accountNumber);  
  }  
  
  @PutMapping(value = "/accounts/{id}/change",  
  produces = MediaType.APPLICATION_JSON_VALUE)  
      Account changeBalance(@PathVariable("id") Long id,  
      @RequestParam("diff") Double diff) {  
        return balanceService.change(id, diff);  
      }  
  }  
  
```  
```xml  
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"  
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd">  
  
    <changeSet id="account" author="Author">  
        <preConditions onFail="MARK_RAN">  
            <not>  
                <tableExists tableName="account"/>  
            </not>  
        </preConditions>  
  
        <createTable tableName="account" remarks="Счета">  
            <column name="id" type="bigint" remarks="Идентификатор записи">  
                <constraints primaryKey="true"  
                             primaryKeyName="account_pk"  
                             nullable="false"/>  
            </column>  
            <column name="account_number" type="varchar(50)"/>  
            <column name="bic" type="varchar(9)"/>  
            <column name="amount" type="numeric(17,2)"/>  
        </createTable>  
    </changeSet>  
</databaseChangeLog>  
```  
```xml  
<databaseChangeLog xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog  
http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.1.xsd">  
  
    <include file="01_add_account.xml" relativeToChangelogFile="true"/>  
</databaseChangeLog>  
```  
  
#### Задача 19 
  
```java  
/*  
    задача : сделать консольное приложение, которое должно читать csv-файл и выводить данные в заданном формате. Мы так же хотим сохранять данные в промежуточную структуру,чтоб дальше обрабатывать. Разработчик выполнил эту задачу, проведи код ревью.  
    Какие есть замечания по коду?  
      
    Адыгейск;Адыгея;Южный;12248;1973;  
    Майкоп;Адыгея;Южный;144246;1857;  
    Горно-Алтайск;Алтай;Сибирский; 5698;1830;  
    Алейск;Алтайский край;Сибирский;29512;1913;  
    Бийск;Алтайский край;Сибирский; 210055;1709;  
    Горняк;Алтайский край;Сибирский;13924;1942;  
    Славгород;Алтайский край;Сибирский;32390;1910;  
    Яровое;Алтайский край;Сибирский;18605;1943;  
*/  
  
public class Main{  
  
    //необходимо считать данные из файла cities.csv и выыести на экран в следующем формате  
    // Город: Сергач, регион: Нижегородская область, федеральный округ: Приволжский, население :21322, основан: 1649  
  
    static City[] cityArray = {};  
  
    public static void main(String[] args){  
        getCityListFromFile();  
        printCititesName();  
    }  
  
    public static void getCityFromFile(){  
        BufferedReader reader;  
        try{  
            reader = new BufferedReader(new FileReader("src/cities.csv"));  
            String line;  
            do{  
                line = reader.readLine();  
                addCityToArray(line);  
            } while(line!=null) {  
                reader.close();  
            }  
        } catch(IOException e){  
            e.printStackTrace();  
        }  
    }  
  
    public static void addCityToArray(String line){  
        String splitLine;  
        int population;  
        int foundation;  
        try{  
            splitLine = line.split(";");  
            if(splitLine.length != 6) throw new Exception("Error line!");  
            population = Integer.parseInt(splitLine[4]);  
            foundation = Integer.parseInt(splitLine[5]);  
        } catch (Exception e){  
            return;  
        }  
        int arrayLength = cityArray.length;  
        City[] newCityArray = new City[arrayLength + 1];  
  
        int i;  
        for(i=0;i<arrayLength;i++) {  
            newCityArray[i] = cityArray[i];  
            newCityArray[i] = new City(splitLine[1], splitLine[2], splitLine[3], population, foundation);  
            cityArray = newCityArray;  
        }  
    }  
  
    public static void printCitiesName(){  
        for(City c: cityArray){  
            System.out.printf("Город: %s, федеральный округ: %s, население: %d, основан: %d\n\n”,  
            c.name, c.region, c.district, c.population, c.foundation );  
        }  
    }  
  
    public class City{  
  
        String name;  
        String region;  
        int population;  
        int foundation;  
    }  
}  
```

#### Задача 20

```java
@Aspect
@RequiredArgsConstructor
public class MyCustomAspect {

    private final MyLogService myLogservice;

    @After (value - execution(* org.ru.test.service.MyWorkService.test(•+)) && args(ato)*, argNames = "dto")
    public void logTraceRabbitReceive(ProceedingJoinPoint pjp, MyDto message) {
        myLogService.logMessage(dto)
    }
}

1. message -> dto
2. have to return Object from pjp.proceed();
3. ProceedingJoinPoint work only with @Around
4. @Component
```

#### Задача 21

```java
Service
public class SampleService {

	private List<String> data = new ArrayList<>();
	@Autowired
	private SomeOtherService1 someOtherService1;
	@Autowired
	private SomeOtherService2 someOtherService2;
	
	public SampleService() {
		someOtherService1.doSomething(number);
	}
	public static int number = 10;
	@Cacheable("dataCache")
	public List<String> processData1 (String input) {
		data.add(input);
		return processData2(data);
	}
	@Transactional
	private List<String> processData2(List<String> data) {
		return someOtherService2.doSomething(data);
	}
}
```