public static void fun1() {
    String str . "Hello";
    System.out.println(str);
    fun2(str);
    System.out.println(str);
}
public static void fun2(String str) {
    str += "World";
}
public static void main(String[ ] args) throws Exception {
    SpringApplication.run(StringTestiApplication.class, args);
    fun1():
}

--------------------------------------------------------------------

public static void fun1() {
    String str1 - "Hello";
    String str2 = "Hello";
    String str3 = new String("Hello");
    System.out.println(str1 == str2);
    System.out.println(str1 == str3);
    System. out.println(str1.equals(str3));
}
--------------------------------------------------------------------

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
--------------------------------------------------------------------

/*
1. Создать отдельный сервис
2. Lazy
3.
*/

--------------------------------------------------------------------

@Component
public class A {
    public void methodA() {
        //some logic
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
--------------------------------------------------------------------

/*
1. @Autowired is in AutowiredAnnotationPostProcessor, but constructor is working before PostProcessors
*/

--------------------------------------------------------------------

interface C {}

@Component
public class A implements C {
}

@Component
public class B implements C {
}

@Component
@RequiredArgsConstructor
public class D {
    private final C c;
}
--------------------------------------------------------------------

void foo() throws Exception {
    //какое исключение будет выброшено из блока?
    try{
        throw new Exception("1");
    finally {
        throw new Exception("2");
    }
}

--------------------------------------------------------------------
public static Supplier<Integer> incrementer(int start) {
    return () -> start++;
}

/*
1. start Have to be effectively final
*/
--------------------------------------------------------------------
//написать функцию которая сравнить два массива
public static boolean compare(int[] arri, int[] arr2) {

}
--------------------------------------------------------------------

public static List<String> foo(List<String> strings) {
    // удалить из коллекции все строки, начинающиеся на абс, без создания новой коллекции и без јача8, без предикатов

}

--------------------------------------------------------------------
Правильной скобочной последовательностью называется строка, состоящая только из символов "скобки" (открывающих ( и закрывающих ) ),
где каждой закрывающей скобке найдётся соответствующая открывающая.
Например, () и ()() - правильные последовательности, а ()() или )( - нет.
Напишите функцию bracketCheck(), которая проверяет,
является ли поступившая на вход строка правильной скобочной последовательностью. Если да, она должна печатать TRUE, в противном случае - FALSE. Обратите внимание,
что пустая строка также является правильной скобочной последовательностью.
Пример 1 Ввод () Вывод TRUE Пример 2 Ввод (()(( Вывод FALSE

System.out.println(bracketCheck("")); // 1 - true
System.out.println(bracketCheck("()")); // 2 - true
System.out.println(bracketCheck("(({}[()]))")); // 3 - true
System.out.println(bracketCheck("(()")); // 4 - false
System.out.println(bracketCheck("((]")); // 5 - false
System.out.println(bracketCheck("]")); // 6 - false

--------------------------------------------------------------------





--------------------------------------------------------------------




--------------------------------------------------------------------