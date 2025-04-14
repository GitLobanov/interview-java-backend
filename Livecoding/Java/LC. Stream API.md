
```java
@Data
@AllArgsConstructor
public static class Book
    private String name;
    private List<String> authors;
    private Double price;
}

// 1. Получить список дорогих книг стоимостью более 500.0
// 2. Получить список всех уникальных авторов в дорогих книгах
// 3. Рассчитать общую стоимость всех дорогих книг
public static void main(String[] args) {
    List<Book> books = Arrays.asList(
            new Book("Book1", List.of("Author1", "Author2"), 800.0),
            new Book("Book2", List.of("Author1", "Author2"), 600.0),
            new Book("Book3", List. of("Author3"), 200.0)
    );

}

```

Решение: BookStream

------------------------------------------------------------------------------------------------

```java
public class CollectionTest {
    @Data
    @AllArgsConstructor
    public static class Student {
        private String lastName;
        private String firstName;
        private String middleName;
        private String specialty;
        private boolean admission;

    }

    public static void main(String[] args) {
        List<Student> students = new ArrayList<>(Arrays.asList(
            new Student("Иванов", "Иван", "Иванович", "Математика", false),
            new Student("Петров", "Петр", "Петрович", "Физика", false),
            new Student("Сидоров", "Сидор", "Сидорович", "Информатика", false))
        );

        // 1. Выполнить сортировку списка абитуриентов в алфавитном порядке по убыванию

        // 2. В случайном порядке проставить признак поступления

        // 3. Вывести список абитуриентов и их количество

        // 4. Отдельным списком вывести всех поступивших+специальность и НЕ поступивших

        System.out.println("Поступившие:");
        //
        System.out.println("НЕ поступившие:");
        //
    }
}
```
Решение: CollectionStudents
------------------------------------------------------------------------------------------------

```java
/*
    Есть список целых чисел, которые встречаются более 2 раз,
    Необходимо вывести пары чисел:
    <число из списка> –< сколько раз число встречается>
    Пример : int mas= [10,15,23,10,15,10,66,10,66,15] -->
    10 ->4
    15 ->3
 */
```


--------------------------------------------------------------------------------------------------
```java

class StudentMark {
    private final int id;
    private final int mark;

    public StudentMark(int id, int mark) {
        this.id = id;
        this.mark = mark;
    }

    public int getId() {
        return id;
    }

    public int getMark() {
        return mark;
    }
}

public class TopStudentsByAverage {
    public static void main(String[] args) {
        List<StudentMark> studentMarks = List.of(
                new StudentMark(1, 5),
                new StudentMark(1, 4),
                new StudentMark(2, 3),
                new StudentMark(2, 4),
                new StudentMark(3, 5),
                new StudentMark(3, 5),
                new StudentMark(4, 4),
                new StudentMark(4, 3),
                new StudentMark(5, 5),
                new StudentMark(5, 5),
                new StudentMark(6, 3),
                new StudentMark(6, 2)
        );

        // а) Нужно с помощью стрима найти топ 5 студентов с лучшей оценкой
        // б) Нужно найти топ 5 студентов с лучшей средней оценкой

    }
}


```
---------------------------------------------------------------------------------------------------

```java
class AnimalBigStream {

    static abstract class Animal {
        private int legs;

        public Animal(int legs) {
            this.legs = legs;
        }

        public int getLegs() {
            return legs;
        }

        public void walk() {
            System.out.println(
                    String.format("Animal with Xd legs is walking now ... ", legs)
            );
        }

        public abstract void eat();
    }

    interface Pet {

        void setName(String name);

        String getName();

        void play();

        static void playIfPet(Animal animal) {
            if (animal instanceof Pet) {
                ((Pet) animal).play();
            }
        }
    }

    static class Cat extends Animal implements Pet {
        private String name;

        public Cat() {
            this("Garfield");
        }

        public Cat(String name) {
            super(4);
            this.name = name;
        }

        @Override
        public void setName(String name) {
            this.name = name;
        }

        @Override
        public String getName() {
            return name;
        }

        @Override
        public void eat() {
            System.out.println(String.format("%s is eating now ... ", name));
        }

        @Override
        public void play() {
            System.out.println(String.format("%s is playing now ... ", name));
        }
    }

    static class Fish extends Animal implements Pet {
        private String name;

        public Fish(String name) {
            super(0);
            this.name = name;
        }

        @Override
        public void setName(String name) {
            this.name = name;
        }

        @Override
        public String getName() {
            return name;
        }

        @Override
        public void play() {
            System.out.println(String.format("%s is playing now ... ", name));
        }

        @Override
        public void eat() {
            System.out.println(String.format("%s is eating now ... ", name));
        }

        @Override
        public void walk() {
            System.out.println(String.format("%s is swimming now ... ", name));
        }
    }

    static class Spider extends Animal {
        public Spider() {
            super(8);
        }

        @Override
        public void eat() {
            System.out.println("Spider is eating now ... ");
        }
    }

    public static List<Animal> wildAnimals(List<Animal> animals) {
        return null;
    }

    public static Animal withHighestNumberOfLegs(List<Animal> animals) {
        return null;
    }

    public static List<Animal> hundredRandomAnimals() {
        return null;
    }

    public static Long totalNumberOfLegs(List<Animal> animals) {
        return null;
    }

    public Map<Integer, List<Animal>> byLegsNumber(List<Animal> animals) {
        return null;
    }//comparator, collectors

    public static Map<Class, Long> bySpecies(List<Animal> animals) {
        return null;
    }

    public static Set<Integer> findDuplicates() {
        IntStream.of(5, 13, 4, 21, 13, 27, 2, 59, 59, 34);
        return null;
    } //Optional, comparator,

    public static Map<String, Long> wordFreqMap() {
        String str = "Delight in the moment, seek no tomorrow. Wandering souls, seeking the truth, riddle of life.";
        return null;
    }

    public static String petsNames(List<Pet> pets) {
        return null;
    }

    public static void main(String[] args) {
        Cat cat1 = new Cat("Whiskers");
        Cat cat2 = new Cat("Mittens");
        Fish fish1 = new Fish("Nemo");
        Fish fish2 = new Fish("Dory");
        Spider spider1 = new Spider();
        Spider spider2 = new Spider();

        List<Animal> animals = List.of(cat1, cat2, fish1, fish2, spider1, spider2);
        List<Pet> pets = List.of(cat1, cat2, fish1, fish2);

        boolean allTestsPassed = true;

        // Тестируем wildAnimals()
        List<Animal> wild = AnimalBigStream.wildAnimals(animals);
        if (wild.size() != 2) {
            System.err.println("ОШИБКА: wildAnimals() должен вернуть 2 элемента, получили " + wild.size());
            allTestsPassed = false;
        }
        if (!wild.isEmpty() && !(wild.get(0) instanceof Spider)) {
            System.err.println("ОШИБКА: wildAnimals() должен возвращать пауков");
            allTestsPassed = false;
        }

        // Тестируем withHighestNumberOfLegs()
        Animal mostLegs = AnimalBigStream.withHighestNumberOfLegs(animals);
        if (mostLegs == null || mostLegs.getLegs() != 8) {
            System.err.println("ОШИБКА: withHighestNumberOfLegs() должен вернуть паука с 8 ногами");
            allTestsPassed = false;
        }

        // Тестируем hundredRandomAnimals()
        List<Animal> randomAnimals = AnimalBigStream.hundredRandomAnimals();
        if (randomAnimals.size() != 100) {
            System.err.println("ОШИБКА: hundredRandomAnimals() должен вернуть 100 животных");
            allTestsPassed = false;
        }

        // Тестируем totalNumberOfLegs()
        Long totalLegs = AnimalBigStream.totalNumberOfLegs(animals);
        if (totalLegs == null || totalLegs != 24L) {
            System.err.println("ОШИБКА: totalNumberOfLegs() должен вернуть 24, получили " + totalLegs);
            allTestsPassed = false;
        }

        // Тестируем byLegsNumber()
        AnimalBigStream animalStream = new AnimalBigStream();
        Map<Integer, List<Animal>> byLegs = animalStream.byLegsNumber(animals);
        if (byLegs.get(0).size() != 2) {
            System.err.println("ОШИБКА: должно быть 2 животных с 0 ногами");
            allTestsPassed = false;
        }
        if (byLegs.get(4).size() != 2) {
            System.err.println("ОШИБКА: должно быть 2 животных с 4 ногами");
            allTestsPassed = false;
        }
        if (byLegs.get(8).size() != 2) {
            System.err.println("ОШИБКА: должно быть 2 животных с 8 ногами");
            allTestsPassed = false;
        }

        // Тестируем bySpecies()
        Map<Class, Long> bySpecies = AnimalBigStream.bySpecies(animals);
        if (bySpecies.get(Cat.class) != 2L) {
            System.err.println("ОШИБКА: должно быть 2 кота");
            allTestsPassed = false;
        }
        if (bySpecies.get(Fish.class) != 2L) {
            System.err.println("ОШИБКА: должно быть 2 рыбы");
            allTestsPassed = false;
        }
        if (bySpecies.get(Spider.class) != 2L) {
            System.err.println("ОШИБКА: должно быть 2 паука");
            allTestsPassed = false;
        }

        // Тестируем findDuplicates()
        Set<Integer> duplicates = AnimalBigStream.findDuplicates();
        if (duplicates.size() != 2) {
            System.err.println("ОШИБКА: должно быть найдено 2 дубликата");
            allTestsPassed = false;
        }
        if (!duplicates.contains(13)) {
            System.err.println("ОШИБКА: должен быть найден дубликат 13");
            allTestsPassed = false;
        }
        if (!duplicates.contains(59)) {
            System.err.println("ОШИБКА: должен быть найден дубликат 59");
            allTestsPassed = false;
        }

        // Тестируем wordFreqMap()
        Map<String, Long> wordFreq = AnimalBigStream.wordFreqMap();
        if (wordFreq.get("the") != 3L) {
            System.err.println("ОШИБКА: слово 'the' должно встречаться 3 раза");
            allTestsPassed = false;
        }
        if (wordFreq.get("seeking") != 1L) {
            System.err.println("ОШИБКА: слово 'seeking' должно встречаться 1 раз");
            allTestsPassed = false;
        }

        // Тестируем petsNames()
        String names = AnimalBigStream.petsNames(pets);
        if (!names.contains("Whiskers")) {
            System.err.println("ОШИБКА: имена должны содержать 'Whiskers'");
            allTestsPassed = false;
        }
        if (!names.contains("Nemo")) {
            System.err.println("ОШИБКА: имена должны содержать 'Nemo'");
            allTestsPassed = false;
        }

        if (allTestsPassed) {
            System.out.println("Все тесты пройдены успешно!");
        } else {
            System.out.println("Обнаружены ошибки в тестах!");
        }
    }
}
```




