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

> Решение: [LC_FillChildrenInTreeNodeForTreeList](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_FillChildrenInTreeNodeForTreeList.java)
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

> Решение: [LC_FindAInListByBestAlgo](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_FindAInListByBestAlgo.java)
#### Задача 3

Дан список строк {"a", "bb", "ccc", "dddd"}. Создайте новый список, содержащий только строки
длиной более 2 символов.

```java
private static void solution(List<String> list) {
	// TODO
}
```

> Решение: [LC_CreateListWithMoreThanTwoSymboles](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_CreateListWithMoreThanTwoSymboles.java)
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

Решение: [LC_RemoveDuplicatesInSortedArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_RemoveDuplicatesInSortedArray.java)
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

Решение: [ParseHolder](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/service/ParseHolder.java)
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

public int[] twoSum(int[] nums, int target) {

}
```

Решение: [LC_FindSumOfTwoInSortedArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/LC_FindSumOfTwoInSortedArray.java)


#### Задача 7

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
#### Задача 8

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

Решение: [LC_ValidateBrackets](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_ValidateBrackets.java)
#### Задача 9

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

Решение: [LC_LongestCommonPrefix](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_LongestCommonPrefix.java)
#### Задача 10

```java
//Нужно написать метод, который переворачивает значение Integer:
//120 -> 21
//351 -> 153

public static int reverse(int i){
    //
}
```

Решение: [LC_ReverseInteger](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_ReverseInteger.java)

#### Задача 11

```java

// Отсортировать сначала четные, затем нечетные
// List<Integer> list = List.of(1, 2, 5, 3, 6, 2); // Результат: 2 2 6 1 3 5

public List<Integer>sortEvenAndOdd (List<Integer> list){

}
```

Решение: [LC_DivideStreamDigitsToOddAndEven](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_DivideStreamDigitsToOddAndEven.java) 

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

Решение: [LC_BubbleSort](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/sort/LC_BubbleSort.java)
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
```

Решение: [LengthOfLongestSubstring](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_LengthOfLongestSubstring.java)

#### Задача 14

```java
/**
* Дано целое число x, вернуть true если x является палиндромом, и false в ином случае.
*/

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

public boolean isIntegerPalindrome(int x) {

}
```

Решение: [LC_IsIntegerPolyndrome](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_IsIntegerPolyndrome.java) 
#### Задача 15

```java

// Инвертировать входящий массив
// {1, 2, 3, 4, 5}

public int[] arrayReverse (int[] array) {
    //
}
```

Решение: [LC_ArrayReverse](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/LC_ArrayReverse.java)
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

Решение: [LC_MaxMinArrays](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/LC_MaxMinArrays.java)
#### Задача 17

```java
/**
* Дана String s, возвратите самую длинную палиндромную подстроку из s
* Пример 1:
* Входные данные: s = "babad"
* Выходные данные: "bab"
* Объяснение: "aba" так же является правильным ответом.
*
* Пример 2:
* Входные данные: s = "cbbd"
* Выходные данные: "bb"
*/
public String longestPalindrome(String s) {

}
```

Решение: [LC_LongsetPalindromeInString](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_LongsetPalindromeInString.java)
#### Задача 18

Пример: 
```java
/**
* Поменять местами два значение переменных, не используя дополнительные переменные
* b = 7, a = 8 -> b = 8, a = 7
**/
public static void main(String[] args) {
	int a = 7;
	int b = 8;

	System.out.println("a: " + a + " b: " + b);
}
```

Решение: [LC_SwapValueBetweenTwoVariablesWithoutThirdValue](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/math/LC_SwapValueBetweenTwoVariablesWithoutThirdValue.java)
#### Задача 19 Найти уникальное число

```java
// Найти уникальное число
// Решить без использования других структур данных, минимум за O(n)
int[] nums = {1, 1, 1, 2, 3, 3, 4, 4, 8, 8}; // 2
int[] nums = {1, 1, 1, 2, 2, 3, 3, 4, 4, 8, 8, 9}; // 9
int[] {1} // 1
int[] {1,2} // 1
null // 0
int[] {} // 0
private static int unique(nums int[]) {
	return 0;
}
```

> Решение: [LC_FindUniqueNumberInArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/LC_FindUniqueNumberInArray.java)
#### Задача 20

```java
/*
Необходимо реализовать метод, который будет возвращать итоговую сумму списания с банковского счета с учетом комиссии за перевод средств.
Необходимо избегать конструкции выбора (if-else, switch-case, filter и Т.д.).
Известно что на вход приходят валидные данные. Сумма не может быть меньше 0, но может быть 0.
Комиссии за суммы:
... < 1_000р - 0%
1 000р - 1%
10 000р - 2%
100 000р < ... - 5%
*/
public static BigDecimal countTotalSum(BigDecimal transferSum) {
	return null;
}
```

> Решение: [LC_СalculatingСommission](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/service/LC_СalculatingСommission.java)
#### Задача 21

```java
// Дана последовательность чисел. Нужно её схлопнуть в строку.
// [6,1,2,3, 7,0, 9] => "0-3, 6-7, 9". Числа неотрицательные.
public String getRange(int[] numbers) {    
    return "";
}
```

> Решение: [LC_FormatToRange](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/string/LC_FormatToRange.java)
#### Задача 22

```java
/*
Написать метод поиска в массиве ближайшего к 10 числа
*/
public static void main(String[] args) {
        System.out.println(findClosestToTen(new int[]{-15, -35, 200, 29, 78}));  // 29
        System.out.println(findClosestToTen(new int[]{9, 12}));                  // 9 (или 12 - одинаково близки)
        System.out.println(findClosestToTen(new int[]{10, 5, 15}));              // 10
        System.out.println(findClosestToTen(new int[]{}));                       // 0
}

private static int findClosestToTen(int[] array) {
	return 0;
}
```

Решение: [LC_FindClosestToTen](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_FindClosestToTen.java)
#### Задача 24

```java
/*
Найти второе максимально число
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

> Решение: [LC_FindSecondMaxInArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/LC_FindSecondMaxInArray.java)
#### Задача 25

```java
// строка, считаем символы, выводим кол-во вхождений каждого
public String getSummary (String string) {
	return "a - 0";
}
```

> Решение: [LC_SummaryBySymbols](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/algorithm/LC_SummaryBySymbols.java)
#### Задача 26

```java
// проверить все ли символы являются буквами
public boolean isAllLetters (String string) {
	return false;
}
```

> Решени: [LC_IsAllLetters](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/regex/LC_IsAllLetters.java)
#### Задача 27

```java
/**
* Найти первый неповторяющийся элемент в массиве
*/
public static void main(String[] args) {
        int[] nums = {4, 5, 1, 2, 0, 4, 5, 2};
}        
```

[FindFirstNotRepeatElementInArray](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/array/LC_FindFirstNotRepeatElementInArray.java)
#### Задача 28

```java
/**
* Проверить, является ли строка палиндромом?
*/
public static void main(String[] args) {  
    String str = "A man a plan a canal Panama";  
    System.out.println("Is palindrome: " + isPalindrome(str)); 
}

public static boolean isPalindrome(String str) {
	// TODO
	return true;
}
```

[LC_IsStringPalindrome](https://github.com/GitLobanov/java-live-coding-one/blob/master/src/main/java/by/lobanov/training/ru/livecoding/core/string/LC_IsStringPalindrome.java)

#### Задача 29


