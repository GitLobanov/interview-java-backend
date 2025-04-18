Задача 1
Сделать ревью



2. Задача
Какой count мы получим если 1000 пользователей попадут на getAdd

@RestController
public class NyController {

    int count = 0;

    @CetMapping("/getAdd)
    public void getAdd () {
        count++;
    }

    @GetMapping("/get")
    public int get () {
        return count;
    }
}

---------------------------------------------------------------------
3. Задача
Как заинжектить прототайп в синглтон

@Component
@Scope(name = "prototype", description =
public class TestBean Prototype {
}

@Service
@RequiredArgsConstructor
public class TestBean {
    private final TestBean Prototype test Beam Prototype;
    public TestBean Prototype getTestBean Prototype()
    return testBean Prototype;
}

@SpringBootApplication
public class Main {

    public static void main(String() args) {
        ApplicationContext context = SpringApplication.run(Main.class, args);
        var singleton Bean First = context.getBean(TestBean.class);
        var prototype Bean First = singleton Bean First.getTestBean Prototype();
        var singleton Bean Second = context.getBean(TestBean.class);
        var prototype Bean Second = singleton Bean Second.getTestBean Prototype();
        System.out.println(prototypeBeanFirst.equals(prototypeBeanSecond));
    }
}

4. Задача

@Data
public class User {

    private String name;
    private Integer age;
    private List<Groups> groups;
}

@Data
public class Groups {

    private String name;
    private String description;
}

//вернуть список usex у которых есть хоть одна группа, которая начинается на "x"
public List<User> getUsers(Stream<User> userStream) {

}

5. Задача


6. Задача

//сгруппировать слова, в которых все буквы одинаковые
//input: ["eat", "tea", "tan", "ate", "nat", "bat"]
//output: [[eat, tea, ate], [bat], [tan, nat]]
public static List<List<String>> group Words(List<String> words){

}


Решение
public static List<List<String>> group Words(List<String> words){
    List list = new ArrayList();
    Map<String, List<String>> map = new HashMap();

    for (String word: words) {
        String sortedWord = words.chars().sorted().mapToObj(String::valueOf)collect(Collector.joining());
        map.putIfAbsent(sortedWord, new ArrayList());
        map.get(sortedWord).add(word);
    }

    return List.of(map.values())
}