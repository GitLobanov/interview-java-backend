#### RC 1

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

#### RC 2

