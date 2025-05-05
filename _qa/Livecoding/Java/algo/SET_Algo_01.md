#### Задача 1

```java
// Заполнить поле children у каждого объекта в списке treeList. Как это сделать?  
static class Node {  
    long id; //заполнен  
    Long parentId; //заполнен  
    List<Node> children; //пустой  
  
    public Node(long id, Long parentId) {  
        this.id = id;  
        this.parentId = parentId;  
    }
}

public static void main(String[] args) {  
    TheNodeChildren fill = new TheNodeChildren();  
    List<Node> list = fill.fillChildren();  
    for (Node node : list) {  
        System.out.println("Node ID: " + node.id + ", Children: " + node.children.size());  
    }
}

// Реализовать
private static List<Node> fillChildren () {
	List<Node> treeList = getList();
	return treeList;
}

private static List<Node> getList() {  
    List<Node> nodes = new ArrayList<>();  
  
    nodes.add(new Node(1, null));  // Корневой узел  
    nodes.add(new Node(2, 1L));    // Дочерний узел для 1  
    nodes.add(new Node(3, 1L));    // Дочерний узел для 1  
    nodes.add(new Node(4, 2L));    // Дочерний узел для 2  
    nodes.add(new Node(5, 2L));    // Дочерний узел для 2  
    nodes.add(new Node(6, 3L));    // Дочерний узел для 3  
  
    return nodes;  
}
```

> Решение: [FillChildrenInTreeNodeForTreeList](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/FillChildrenInTreeNodeForTreeList.java)
#### Задача 2

```java
/*
1. Есть класс А
2. Входной список отсортирован по i
3. Нужно найти элемент A.i = 18
4. Использовать бест алгоритм сложности
5. Вернуть один элемент в списке
6. Какая сложность будет подходить если нужно вернуть все возможные вхождения (несколько элементов)
 */
 
public static class A {  
    public Integer i;  
    public String str;  
}

public List<A> findA(List<A> list){
    //
    return null;
}

// hint - binary search
```

> Решение: [FindAInListByBestAlgo](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/FindAInListByBestAlgo.java)
#### Задача 3

Дан список строк {"a", "bb", "ccc", "dddd"}. Создайте новый список, содержащий только строки
длиной более 2 символов.

```java
private static void solution(List<String> list) {
	// TODO
}
```

> Решение: [CreateListWithMoreThanTwoSymboles](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/CreateListWithMoreThanTwoSymboles.java)
#### Задача 4

```java
/* 
Преобразовать отсортированный массив неуникальных чисел вида [1,1,2,2,3,3,6,7] в массив отсортированных по возрастанию уникальных чисел - [1,2,3,6,7,_,_,_]

* без использования коллекций и стримов
 */
// example: [1,1,2,2,3,3,6,7] -> [1,2,3,6,7,_,_,_]

class Solution {
    public void removeDuplicates(int[] nums) {
		
    }
}
```

Решение: [RemoveDuplicatesInSortedArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/RemoveDuplicatesInSortedArray.java)
#### Задача 5


```java
/**
    Задача написать класс с тремя методами
    
    - в метод parse передается строка вида key1=val1;key2=val2;key2=val3... и нужно это все упаковать
    каким то образом чтобы getRecord возвращала начальную строку
    - getValue сложность O(1)
    - getRecord - кэшируем строку в методе parse в переменную String и возвращаем
    - 1 доп - Сделать так, чтобы могли храниться дубликаты значений (решал через HashMap<String, String> переделал -> HashMap<String, List<String>>)
    - 2 доп сделать класс иммутабельным (метод parse = конструктор и все вспомогательные поля private final геттеры не создавать)
*/
    static class ParseHolder {

        public void parse(String str) {

        }

        public String getRecord() {

        }

        // За O(1)
        public String getValue(String key) {

        }

    }
```

#### Задача 6

```java
/**
 * Дан отсортированнй массив целых чисел nums и целое число target,
 * нужно вернуть индексы двух чисел из массива таким образом,
 * чтобы они в сумме давали target.
 * Вы можете вернуть ответ в любом порядке.
 */

/**
 * Пример 1:
 * Входные данные: nums = [2,7,11,15], target = 9
 * Выходные данные: [0,1]
 * Объяснение: Поскольку nums[0] + nums[1] == 9, возвращаем [0, 1].
 *
 * Пример 2:
 * Входные данные: nums = [3,2,4], target = 6
 * Выходные данные: [1,2]
 *
 * Пример 3:
 * Входные данные: nums = [3,3], target = 6
 * Выходные данные: [0,1]
 */

class Solution {
    public int[] twoSum(int[] nums, int target) {

    }
}
```

Решение: [FindSumOfTwoInSortedArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/FindSumOfTwoInSortedArray.java)
#### Задача 7

```java
/*
Дана строка, состоящая из скобок ({[]}), необходимо проверить строку на валидность.
Валидной строка является та, в которой встречается открывающая и соответствующая ей закрывающая скобка.

System.out.println(bracketCheck("")); // 1 - true
System.out.println(bracketCheck("()")); // 2 - true
System.out.println(bracketCheck("(({}[()]))")); // 3 - true
System.out.println(bracketCheck("(()")); // 4 - false
System.out.println(bracketCheck("((]")); // 5 - false
System.out.println(bracketCheck("]")); // 6 - false
 */

private static boolean bracketCheck (String str) {
    return false;
}
```

#### Задача 8


```java

/**
* Есть массив слов
* Написать функцию, которая определяет самый длинный общий префикс
* Пример:
* Массив array{"flower", "flow", "flight"}
* Самый длинный общий префикс -> "fl"
* Массив array {"dog","racecar","car"}
* Самый длинный общий префикс -> ""
*/

```

#### Задача 9

```java
//Нужно написать метод, который переворачивает значение Integer:
//120 -> 21
//351 -> 153

public static int reverse(int i){
    //
}
```

Решение: ReverseInteger

#### Задача 10

```java

// Отсортировать сначала четные, затем нечетные
// List<Integer> list = List.of(1, 2, 5, 3, 6, 2); // Результат: 2 2 6 1 3 5

public List<Integer>sortEvenAndOdd (List<Integer> list){

}
```

#### Задача 11

```java
// Удалить все элементы меньше 3 не используя стримы
```

#### Задача 12

```java
/*
Реализовать пузырьковую сортировку
 */
public class BubbleSort(){
    public static int[] sort (int[] array){ [1, 2, 4, 56, 13, 721, 0] };

    }
}
```

#### Задача 13

```java

/**
* Данa строка s, найте длину самой длинной подстроки (substring)
* без повторяющихся символов
*/
class Solution {
    public static int lengthOfLongestSubstring(String s) {
        // 
    }
}

/**
* Пример 1:
* Входные данные: s = "abcabcbb"
* Выходные данные: 3
* Объяснение: правильный ответ - "abc", с длиной равной 3.
*
* Пример 2:
* Входные данные: s = "bbbbb"
* Выходные данные: 1
* Объяснение: правильный ответ - "b", с длиной равной 1.
*
* Пример 3:
* Входные данные: s = "pwwkew"
* Выходные данные: 3
* Объяснение: правильный ответ - "wke", с длиной равной 3.
*/

// Решение: LengthOfLongestSubstring
```

#### Задача 14

```java

/**
* Дано целое число x, вернуть true если x является палиндромом, и false в ином случае.
*/

class Solution {
    public boolean isPalindrome(int x) {

    }
}

/**
* Пример 1:
* Входные данные: x = 121
* Выходные данные: true
* Объяснение: 121 читается как 121 слева направо и справа налево.
*
* Пример 2:
* Входные данные: x = -121
* Выходные данные: false
* Объяснение: Слева направо читается как -121.
* Справа налево превращается в 121-. И поэтому это не является палиндромом.
*
* Пример 3:
* Входные данные: x = 10
* Выходные данные: false
* Объяснение: Представляет собой 01 справа налево. И поэтому не является палиндромом.
*/
```

#### Задача 15

```java

// Инвертировать входящий массив
// {1, 2, 3, 4, 5}

public int[] arrayReverse (int[] array) {
    //
}
```

#### Задача 16

```java
/*
Дан массив, нужно преобразовать в формат:
- макс число, мин число, второе макс число, второе мин число
Пример: solve([15,11,10,7,12]) = [15,7,12,10,11]
Доп* - не использовать готовые реализации сортировки
*/

public static void main(String[] args) {  
    System.out.println(Arrays.toString(solve(new int[]{15, 11, 10, 7, 12})));  
}  
  
public static int[] solve(int[] arr) {  
	return null; 
}
```

#### Задача 17

```java
/**
* Дана String s, возвратите самую длинную палиндромную подстроку из s
*/
class Solution {
    public String longestPalindrome(String s) {

    }
}

/**
* Пример 1:
* Входные данные: s = "babad"
* Выходные данные: "bab"
* Объяснение: "aba" так же является правильным ответом.
*
* Пример 2:
* Входные данные: s = "cbbd"
* Выходные данные: "bb"
*/
```

#### Задача 18

```java
/**
* Дано 32 битное signed целое число x, "разверните" число х в обратную сторону.
* Если развёрнутое число х имеет значение,
* выходящее за рамки 32 битного signed целого числа [-2e31, 2e31 - 1], тогда, возвратите 0.
*/

class Solution {
    public int reverse(int x) {

    }
}

/**
* Пример 1:
* Входные данные: x = 123
* Выходные данные: 321
*
* Пример 2:
* Входные данные: x = -123
* Выходные данные: -321
*
* Пример 3:
* Входные данные: x = 120
* Выходные данные: 21
*
* Ограничения:
*
* -231 <= x <= 231 - 1
*/
```

#### Задача 19 Поменять местами два значение переменных, не используя дополнительные переменные

Пример: 
```java
// b = 7, a = 8 -> b = 8, a = 7
public static void main(String[] args) {
	int a = 7;
	int b = 8;

	System.out.println("a: " + a + " b: " + b);
}
```

Решение:

1 решение

```java
public class javaXor {
    public static void main(String[] args) {
        // binary of 7=0111
        int a = 7;
        // binary of 8=1000
        int b = 8;
        // swapping using java XOR operator
        // now a is 1111 15 and b is 8
        a = a ^ b;
        //now a is 1111 15 but b is 7 (original value of a)
        b = a ^ b;
        // now a is 8 and b is 7, numbers are swapped
        a = a ^ b;
		System.out.println("a: " + a + " b: " + b);
    }
}
```

#### Задача 20 Найти уникальное число

```java
// Найти уникальное число
// Решить без использования других структур данных, минимум за O(n)
int[] nums = {1, 1, 1, 2, 3, 3, 4, 4, 8, 8}; // 2
int[] nums = {1, 1, 1, 2, 2, 3, 3, 4, 4, 8, 9}; // 9
int[] nums = {3, 3, 7, 7, 10, 11, 11}; // 10
int[] {1}
int[] {1,2}
null
int[] {}
int unique(nums int[]) {
	return null;
}
```

#### Задача 21 

```java
/*
Необходимо реализовать метод, который будет возвращать итоговую сумму списания с банковского счета с учетом комиссии за перевод средств.
Необходимо избегать конструкции выбора (if-else, switch-case, filter и Т.д.).
Известно что на вход приходят валидные данные. Сумма не может быть меньше 0, но может быть 0.
Комиссии за суммы:
< 1_000р - 0%
1 000р - 1%
10 000р - 2%
100_000р - 5%
1000 000р
*/
public static BigDecimal count Total Sum(BigDecimal transferSum) {
	return null;
}
```

#### Задача 22

```java
// Дана последовательность чисел. Нужно её схлопнуть в строку.
// [6,1,2,3, 7,0, 9] => "0-3, 6-7, 9". Числа неотрицательные.
public String getRange(int[] numbers) {    
    return "";
}
```

###### Решение

```java
// Дана последовательность чисел. Нужно её схлопнуть в строку.
// [6,1,2,3, 7,0, 9] => "0-3, 6-7, 9". Числа неотрицательные.
public String getRange(int[] numbers) {
    if(numbers==null || numbers.length<2){
        return "";
    }
    Arrays.sort(numbers);
    StringBuilder result = new StringBuilder();
    
    int start = numbers[0];
    int finish = start;
    
    for(int i = 1; i<numbers.length; i++){

        if (numbers[i]==finish+1){
            finish = numbers[i];
        } else {
            result.append(start).append("-").append(finish);
            
            start=numbers[i];
            finish=start;
            
            if (i<numbers.length-1){
                result.append(", ");
            } else{
                result.append(finsh);
            }
        
        }
        
    }
    
    return result.toString();
}
```


#### Задача 23

```java
public static void main(String[] args) {
	Integer[] input = {1, 2, 5, 4, 4, 5, 2, 3, 6, 5};
	System.out.println(transform(input));
}

/*
В результате нет 4 и 6.
Ключ - преобразованный элемент стрима.
Значение - кол-во вхождений данного элемента в стриме
Пример: "3-hello": 1
*/
public static Map<String, Integer> transform (final Integer... nums) {
	return;
}
```

#### Задача 24 

```java
/*
Написать метод поиска в массиве ближайшего к 10 числа
*/
public static void main(String[] args) {
	System.out.println(findClosestToTen(new int[]{-15, -35, 200, 29, 78}));
}

private static int findClosestToTen(int[] array) {
	return 0;
}
```

Решение: FindClosestToTen
#### Задача 25

```java
/*
Найти второй максимально число
Например: 
[3,9,4,0,8,1] -> 8
[1000,7,12,0,5,3] -> 12
*/
public class FindSecondMax {
	public static void main(String[] args) {
		System.out.println(findSecondMax(new int []{3,9,4,0,8,1}));
		System.out.println(findSecondMax(new int []{1000,7,12,0,5,3}));
	}
	
	public static int findSecondMax(int] [array) {
		return 0;
	}
}
```

Решение: LC_FindSecondMaxInArray

#### Задача 26

```java
// строка, считаем символы, выводим кол-во вхождений каждого
public String getSummary (String string) {
	return "a - 0";
}
```

#### Задача 27

```java
// проверить все ли символы являются буквами
public boolean isAllLetters (String string) {
	return false;
}
```

#### Задача 28

```java
// объеденить слова в одну строку с делиметром
public static String concatAll (String delimiter, String... strings) {
	return "";
}

public static void main(String[] args) {
	System.out.println(concatAll("-", "Goodbye", "Java", "!"));
}
```

#### Задача 29

```java
**
 * Дан неотсортированный массив целых чисел, вернуть индексы двух
 * чисел, сумма которых равна заданному числу.
 * Вы не можете использовать один и тот же элемент дважды.
 * Пример:
 * Given nums - [2, 7, 11, 15], target - 9.
 * The output should be [0, 1].
 * Because nums[0]+nums[1] 2+7=9.
 */
public class FindSumOfTwoInNotSortedArray {

    public static void main(String[] args) {
        System.out.println(Arrays.toString(findTwoOfSum(new int[]{2, 1, 4, 2, 6}, 6)));
    }

    private static int[] findTwoOfSum(int[] nums, int target) {
        return new int[]{};
    }
}
```

Решение: [FindSumOfTwoInNotSortedArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/FindSumOfTwoInNotSortedArray.java)

#### Задача 30