---
Тип:
  - Порождающий
---
### 1. Создаем интерфейс для элементов структуры

```java
// Интерфейс для элементов структуры
interface Element {
    void accept(Visitor visitor);
}
```
### 2. Создаем интерфейсы Итератора и Коллекции

Определим интерфейсы для итератора и коллекции, которую он будет обходить.

```java
// Интерфейс Итератора
interface Iterator<T> {
    boolean hasNext();
    T next();
}

// Интерфейс Коллекции, которая может возвращать итератор
interface Aggregate {
    Iterator<Element> createIterator();
}
```
### 3. Реализуем конкретные элементы структуры

Реализуем простую структуру данных с несколькими элементами.

```java
// Конкретный элемент структуры
class SimpleElement implements Element {
    private String name;

    public SimpleElement(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```
### 4. Реализуем коллекцию с итератором

Создадим коллекцию элементов и итератор для обхода этой коллекции.

```java
// Коллекция элементов, которая возвращает итератор
class ElementCollection implements Aggregate {
    private List<Element> elements = new ArrayList<>();

    public void add(Element element) {
        elements.add(element);
    }

    @Override
    public Iterator<Element> createIterator() {
        return new ElementIterator();
    }

    private class ElementIterator implements Iterator<Element> {
        private int index = 0;

        @Override
        public boolean hasNext() {
            return index < elements.size();
        }

        @Override
        public Element next() {
            return elements.get(index++);
        }
    }
}

```
### 5. Создаем интерфейс Посетителя

Определяем интерфейс посетителя, который будет выполнять действия над каждым элементом.

```java
// Интерфейс Посетителя
interface Visitor {
    void visit(SimpleElement element);
}
```
### 6. Реализуем конкретного Посетителя

Создаем конкретного посетителя, который будет, например, выводить имя элемента.

```java
// Конкретный посетитель
class PrintVisitor implements Visitor {
    @Override
    public void visit(SimpleElement element) {
        System.out.println("Visited element: " + element.getName());
    }
}
```
### 7. Используем Итератор и Посетителя вместе

Теперь можно объединить итератор и посетителя, чтобы обойти все элементы структуры и выполнить действия над каждым из них.

```java
public class Main {
    public static void main(String[] args) {
        // Создаем коллекцию элементов
        ElementCollection collection = new ElementCollection();
        collection.add(new SimpleElement("Element 1"));
        collection.add(new SimpleElement("Element 2"));
        collection.add(new SimpleElement("Element 3"));

        // Создаем итератор для обхода коллекции
        Iterator<Element> iterator = collection.createIterator();

        // Создаем посетителя
        Visitor visitor = new PrintVisitor();

        // Обходим элементы и применяем к ним посетителя
        while (iterator.hasNext()) {
            Element element = iterator.next();
            element.accept(visitor);
        }
    }
}
```
### Результат

Программа выведет:

```C
Visited element: Element 1
Visited element: Element 2
Visited element: Element 3
```

### Как это работает:

1. **Element (Элемент):** Определяет общий интерфейс для элементов структуры, включая метод `accept`, который принимает посетителя.
    
2. **SimpleElement (Конкретный элемент):** Реализует элемент структуры и поддерживает метод `accept`, который передает себя посетителю.
    
3. **Iterator (Итератор):** Определяет интерфейс для обхода элементов структуры.
    
4. **Aggregate (Коллекция):** Определяет интерфейс коллекции, которая может возвращать итератор.
    
5. **ElementCollection (Конкретная коллекция):** Реализует коллекцию элементов и предоставляет итератор для их обхода.
    
6. **Visitor (Посетитель):** Определяет действия, которые будут выполняться над элементами.
    
7. **PrintVisitor (Конкретный посетитель):** Реализует конкретное действие (вывод информации) для каждого элемента.
    
8. **Main (Клиентский код):** Использует итератор для обхода структуры и применяет посетителя к каждому элементу.