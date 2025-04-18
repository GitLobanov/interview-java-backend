## QA
#### Задача 1. Найти первый неповторяющийся элемент в массиве

```java
public static void main(String[] args) {
        int[] nums = {4, 5, 1, 2, 0, 4, 5, 2};
```

[FindFirstNotRepeatElementInArray]
#### Задача 2. Проверить, является ли строка палиндромом?

```java
public static void main(String[] args) {  
    String str = "A man a plan a canal Panama";  
    System.out.println("Is palindrome: " + isPalindrome(str)); 
}

public static boolean isPalindrome(String str) {
	// TODO
	return true;
}
```

[IsStringPalindrome]

#### Задача 3. Найти 2 элемента упорядоченного массива, сумма которых равна заданному числу

```java
public static void main(String[] args) {  
    System.out.println(Arrays.toString(findSumOfTwo(new int[]{0, 1, 3, 4, 5, 6}, 5)));  
}  
  
private static int[] findSumOfTwo(int[] nums, int target) {
	return new int[]{0,0};
}
```

[FindSumOfTwoInSortedArray]
#### Задача 4. Найти два элемента в неупорядоченном массиве, сумма которых равна заданному числу? 

```java
public static void main(String[] args) {  
    System.out.println(Arrays.toString(findTwoSum(new int[]{2, 1, 4, 2, 6}, 6))); 
}  
  
private static int[] findSumOfTwo(int[] nums, int target) {
	return new int[]{0,0};
}
```

[FindSumOfTwoInNotSortedArray]

