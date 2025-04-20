- **Решает проблему инвариантности [[Generics]]**
- Изображается как \<?> и используется для расширения возможностей параметризации в java.
- В отличии от дженериков, могут использовать еще и контравариантность \<? super Type>  
- Могут использоваться для более гибкой параметризации методов в уже параметризированном классе и еще во множестве частных случаев, таких как [[Generics#PECS|PECS]].

## PECS

```java
List<?> persons = ...// unbounded wildcard
List<? extends Employee> employees = ... // upper bounded wildcard
List<? super Employee> managers = ...// // lower bounded wildcard
```

Producer Extends Consumer Super - принцип, который используется при копировании элементов из одной коллекции в другую.  

List\<? extends Number> - producer. Из него можно читать в переменную типа Number и выше по иерархии. Записать в коллекцию ничего нельзя, кроме null.  

```java
List<? extends Number> numbers = new ArrayList<Integer>();
Number num = numbers.get(0); // OK
numbers.add(10); // Ошибка компиляции
numbers.add(null); // OK
```

List\<? super Number> - consumer. В него можно писать все, что наследуется от Number. Читать из него можно только в переменную типа Object.

```java
List<? super Number> numbers = new ArrayList<Object>();
numbers.add(10); // OK
numbers.add(3.14); // OK
Object obj = numbers.get(0); // OK
Number num = numbers.get(0); // Ошибка компиляции
```
## Виды

- `Stack<?> stack`

```java
void dump(Stack<?> stack) {
    while (!stack.isEmpty()) {
        Object o = stack.pop();
        System.out.println(o);
    }
}
```

- Bounded wildcard `Stack<? extends Shape> stack`

```java
void draw(Stack<? extends Shape> stack) {
    while (!stack.isEmpty()) {
        Shape shape = stack.pop();
        shape.draw();
    }
}
```


## Ресурсы

- [Полиморфизм и Generics](https://www.kgeorgiy.info/courses/paradigms/slides/generics.xhtml#(31))
- 