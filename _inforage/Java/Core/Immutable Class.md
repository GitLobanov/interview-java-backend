- **Сделать класс `final`**: Должен быть объявлен как final, чтобы от него нельзя было наследоваться. Иначе дочерние классы могут нарушить иммутабельность.
- **Сделать все поля `private` и `final`**: Все поля класса должны быть приватными в соответствии с принципами инкапсуляции.
- **Инициализировать поля через конструктор**: Делать копию принимаемых объектов и инициализировать состояние.
- **Не предоставлять `setter`-методов**:  Для исключения возможности изменения состояния после инстанцирования, в классе не должно быть сеттеров.
- **Защищать изменяемые объекты внутри класса**: Если класс содержит изменяемые объекты, например массивы или списки, возвращайте их копии, а не оригиналы, или применяйте `Collections.unmodifiableList`. Для полей-коллекций необходимо делать глубокие копии, чтобы гарантировать их неизменность.


Для создания можно использовать такое short cut в  джава:

- record
- value