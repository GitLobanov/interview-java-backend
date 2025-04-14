Задача 1
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

?????????????????????????????????????

Доп. вопросы:
1. Какие есть стратегии генерации?
2. Почему нельзя final?
3. Что используется для создания прокси?
4. Какие есть FetchType?
5. Какой по-умолчанию FetchType для каждого маппинга?
6. Когда возникнет n+1?
7. Как решать n+1?
8. Как я могу обнаружить n+1
9. Когда возникает LazyInitException

?????????????????????????????????????

Задача 2
Что выведет equals?

@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    @Transactional
    public void demonstrateById() {
        User user1 = userRepository.findById(1L).orElse(null);
        User user2 = userRepository.findById(1L).orElse(null);
        System.out.println("Are user1 and user2 the same object? " + (user1 == user2));
    }

    @Transactional
    public void demonstrateByName() {
        User user1 = userRepository.findByUsername("username1").orElse(null);
        User user2 = userRepository.findByUsername("username1").orElse(null);
        System.out.println("Are user1 and user2 the same object? " + (user1 == user2));
    }
}

// ?

public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByUsername(String username);
}

@Entity
@Table(name = "users")
public class User {

    @Id
    private Long id;

    private String username;
    private String email;

    // геттеры и сеттеры
}

???????????????????????????

доп вопросы:
1. Какие есть уровни кэширования
2. По какому значению кэшируется сущность

public interface UserRepository extends JpaRepository<User, Long> {

    @Cacheable("usersByName")
    Optional<User> findByUsername(String username);
}

@Entity
@Table(name = "users")
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class User {

    @Id
    private Long id;

    private String username;
    private String email;

    // геттеры и сеттеры
}

spring.jpa.properties.hibernate.cache.use_second_level_cache=true
spring.jpa.properties.hibernate.cache.use_query_cache=true
spring.jpa.properties.hibernate.cache.region.factory_class=org.hibernate.cache.ehcache.EhCacheRegionFactory

<dependency>
    <groupId>org.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>3.10.8</version>
    <classifier>jakarta</classifier>
</dependency>

https://www.baeldung.com/hibernate-second-level-cache

????????????????????????????????????