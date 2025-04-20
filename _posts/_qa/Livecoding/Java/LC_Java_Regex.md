#### Задача 1

```java

/ *  
* Напишите метод findUniqueWords(String text), который принимает строку текста и возвращает список уникальных слов,  
* отсортированных в алфавитном порядке. Словами считаются последовательности символов,  
* разделенные пробелами или знаками препинания.  
*/  
String text = "Hello, world! Hello Java. Java is great.";  
List<String> uniqueWords = findUniqueWords(text);  
List<String> findUniqueWords(String text) {  
    //TODO:  
}  
```

#### Задача 2 (light)

```java
/*  
Валидировать номер банковской карты  
 */
public static void main(String[] args) {  
    // have to true  
    System.out.println(validateNumber("1234567890123456"));  
    System.out.println(validateNumber("1234-5678-9012-3456"));  
    System.out.println(validateNumber("1234 5678 9012 3456"));  
  
    // have to false  
    System.out.println(validateNumber("1234-5678-9012-345")); // меньше 16 цифр  
    System.out.println(validateNumber("1234 5678 9012 3456 7890")); // больше 16 цифр
    System.out.println(validateNumber("1234_5678_9012_3456")); // неверный разделитель  
}  
  
private static boolean validateNumber (String number) {  
    return false;  
}
```

Решение: [ValidateNumberBankCard]


## Resources

- More Regex Quiz here - [regex101/quiz](https://regex101.com/quiz/1)