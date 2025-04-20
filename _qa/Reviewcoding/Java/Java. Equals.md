#### Задача 1

```java  
  
public static void main(String[] args) {  
    Integer i1 = 128;  
    Integer i2 = 128;
    System.out.println("1 -  " + i1.equals(i2));  
  
    Integer i3 = 128;  
    Integer i4 = i3;
    System.out.println("2 -  " + i3.equals(i4));  
  
    Integer i5 = 128;  
    Integer i6 = i5;
    System.out.println("3 -  " + (i5 == i6));  
  
    Integer i7 = 128;  
    Integer i8 = i7;  
    i7 = 127;
    System.out.println("4 -  " + (i7 == i8));  
  
    Integer i9 = 128;
    System.out.println("5 -  " + i9.equals(128));  
  
    Integer i10 = 129;
    System.out.println("6 -  " + (i10 == 129));  
  
    Integer i11 = 128;
  
    System.out.println("7 - " + (i11.hashCode() == 128.hashCode()));  
  
    Integer i12 = 128;  
    Integer i13 = 128;
    System.out.println("8 -  " + (i12.hashCode() == i13.hashCode()));  
  
    Integer i14 = 128;  
    Integer i15 = 128;
    System.out.println("9 - " + (i14.hashCode().equals(i15.hashCode())));  
  
    Integer i16 = 128;  
    Integer i17 = 128;
    System.out.println("10 - " + (i16 == i17));  
  
    Integer i18 = 127;  
    Integer i19 = 127
    System.out.println("11 - " + (i18 == i19));  
  
    int i20 = 128;  
    int i21 = 128;  
    System.out.println("12 - " + (i20 == i21));  
  
    int i22 = 127;  
    int i23 = 127;
    System.out.println("13 - " + (i22 == i23));  
}  
  
```  
  
#### Задача 2

```java  
public static void counter() {  
    int count = 0;  
    String s1 = "java";  
    String s2 = "java";  
    StringBuilder s3 = new StringBuilder("java");  
    s2. toUpperCase();  
    if (s1 == s2) count++;  
    if (s1.equals(s2)) count++;  
    if (s1 == s3) count++;  
    if (s1.equals(s3)) count++;  
    System. out. println(count);  
}  
```  

#### Задача 3
  
  
```java  
public class MyObjectEquals {  
  
    public static void main (String[] args) {  
        MyObject obj = new MyObject(10);  
        HashSet<MyObject> set = new HashSet<>();  
        set.add(obj);  
        obj.set(1000);  
  
        System. out. println(set.contains(obj));  
        System. out. println(set.contains(new MyObject(10)));  
        System. out. println(set.contains(new MyObject(1000)));  
        System. out. println(Objects.equals(new MyObject(1000), obj));  
        System. out. println(obj.equals(new MyObject(1000)));  
    }  
    static class MyObject {  
        private int i;  
        public MyObject(int i) {  
            set(i);  
        }  
        public void set(int i) {  
            this.i = i;  
        }  
  
        public int hashCode() {  
            return i;  
        }  
  
        @Override  
        public boolean equals(Object object) {  
            if (this == object) return true;  
            if (object == null || getClass() != object.getClass()) return false;  
            MyObject myObject = (MyObject) object;  
            return i == myObject.i;  
        }  
    }  
}  
```  

#### Задача 4


```java  
  
public static void main(String[] args) {  
    String s = "a";  
    s.toUpperCase();  
    System.out.println(s);   
}  
```  

#### Задача 5

```java  
  
public static void main(String[] args) {  
    String a = "A1";  
    String b = new String("A1");  
    String c = "A" + "1"; 
    String d = "123";
    String e = new Integer(123).toString(); 
  
    System.out.println(a == b);  
    System.out.println(a == c);
    System.out.println(a.equals(b));  
    System.out.println(d == e);
}  
```