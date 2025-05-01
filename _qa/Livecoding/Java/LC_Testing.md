```java
@Component
public class A {
	@Autowired
	private B b;
	@Autowired
	private C c;
	
	public String methodA () {
		// некая логика работы method:
		String resultB = b.methodB();
		return c.methodC (resultB);
		}
	}
	
public class TestA {
	@Autowired
	private B b;
	
	@Test
	void testMethodA() {
		// тестирование метода, мок бинов в и с
	}
}
```