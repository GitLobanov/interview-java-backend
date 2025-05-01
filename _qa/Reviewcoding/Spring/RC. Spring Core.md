#### Задача 1

```kotlin  
/**  
 * По какому http методу будет доступен url /documents/hello ?   
 */  
@RestController  
class HelloController {  
  
    @RequestMapping("/documents/hello")  
    fun sayHello () : String {  
        return "Hello from Document service in Kotlin";  
    }  
}  
```

#### Задача 2

//Что будет выведено в консоль?

@Component 
public class A {

    public void methodA() { 
        System.out.println("methodA");
    }

}

@Component
public class B {

    @Autowired 
    public A a;

    public B() {
        a.methodA();
    }
}

#### Задача 3

@Component
public class A {

    @Autowired
    public B b;
}

@Component
public class B {

    @Autowired
    public A a;
}

#### Задача 4

@Service 
public class A {

    public void method1() {
        method2();
    }

    @Transactional
    private void method2() {
        // ..
    }
}

@Service
@RequiredArgsConstructor
public class B {

    private final A a;

    public void call() {
        a.method1();
    }
}

#### Задача 5

interface C {}

@Component
public class A implements C {
}

@Component
public class B implements C {
}

@Component
@RequiredArgsConstructor
public class D{

    private final C c;
}
