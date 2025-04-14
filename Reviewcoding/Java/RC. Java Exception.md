#### RC 1 - Что будет в результате?

```java
try {
	return Integer.parseInt(number);
} catch (Exception e) {
	throw new RuntimeException("Ошибка", e);
} finally {
    return 0;
}
```

#### RC 2

```java  
/**  
 насколько корректный тут код?  
 какая иерархия исключений?  
 Spring если в методе выбрасывается checked исключение что будет с транзакцией? что будет с данными которые были записаны до IOException?  
 Throwable это какой тип данных?  
 */  
class GenericExceptions {  
    static class GenericException<T> extends RuntimeException {  
        private T info;  
  
        public GenericException(String message, T info) {  
            super(message);  
            this.info = info;  
        }  
    }  
  
    public static void main(String[] args) {  
        try {  
            doLogic();  
        } catch (Exception unexpectedEx) {  
            handleUnexpectedException(unexpectedEx);  
        } catch (GenericException<DbConnectionInfo> | GenericException<NotFoundInfo> |  
                 GenericException<HttpInfo> genericEx) {  
            handleGenericException(genericEx);  
        }  
    }  
}  
  
public class OomTask {  
    static Optional<byte[]> tryallocateImageBuffer(int size) {  
        try {  
            return Optional.of(new byte[size]);  
        } catch (OutOfMemoryError error) {  
            error.printStackTrace();  
            return Optional.empty();  
        }  
    }  
  
    public static void main(String[] args) {  
        if (tryAllocateImageBuffer(Integer.MAX_VALUE).isPresent()) {  
            System.out.print.ln("Image buffer has been allocated");  
        } else {  
            System.out.print1n("Image buffer hasn't been allocated");  
        }  
    }  
}  
```  


#### RC 3


```java  
// Как отработает следующий код?  
public static void main(String[] args) {  
    byte[] arr;  
    try {  
        arr = new byte[Integer.MAX_VALUE + Integer.MAX_VALUE + Integer.MAX_VALUE];  
    } finally {  
        System.out.println("finally block");  
    }  
    System.out.println("done");  
}  
```

#### RC 4

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
  
    /*  
    Java выбирает наиболее конкретный (специфичный) тип из возможных вариантов.  
    Object  
     └── Throwable  
          └── Exception  
               └── IOException  
                    └── FileNotFoundException  
     */  
  
```  

#### RC 5
  
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

#### RC 6