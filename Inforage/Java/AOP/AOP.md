---
Определение: аспектно-ориентированное программирование, суть которого сводится к созданию сквозной функциональности (например логирование, аудит и пр.).
Примечание:
---
## Join Point

 Место в коде, в выполнение которого мы «вмешиваемся» (и начинаем выполнять что-то своё). В теории может соответствовать вызовам методов, инициализации классов и инстанцированию объектов, но в Spring AOP — это всегда _вызов метода_.

Join point - точки кода, где планируется внедрение функциональности. Определяет, когда исполнять advice.

## Advice

Набор инструкций, исполняемых на точках среза (pointcut).

Код, который «впрыскивается» и выполняется в joinpoint.

- before - перед вызовом метода
- after - после вызова метода
- after returning - после возврата значения метода
- after throwing - после прокидывания исключения
- after finally - в случае выполнения блока finally
- around - до и после вызова метода

```java
@Before("callAtMyServiceAnnotation()")    
public void beforeCallAt() { } 

@Before("callAtMyServiceMethod1(list)")    
public void beforeCallAtMethod1(List<String> list) { }
```
## Pointcut

Pointcut - предикат, который определяет где использовать advice.

Тем или иным способом определённое множество joinpoint-ов. Например, «все методы, начинающиеся со слова get». Или: «все методы, аннотированные аннотацией `@Benchmarked`». Связывая pointcut-ы c advice-ами, мы определяем, что именно и когда будет работать.


```java
@Pointcut("execution(* com.example.demoAspects.MyService.check())")
public void callAtMyServiceAfterReturning() { }
```

## Aspect

Модуль, в котором собраны точки среза и эдвайсы

Комбинация **advices+pointcuts**, оформленная в виде отдельного класса. Определяет добавляемую в приложение логику, ответственную за какую-то определённую задачу (например, трассировку).

```java
@Aspect
@Component
public class MyAspect {

    @Pointcut("execution(public * com.example.demoAspects.MyService.*(..))")
    public void callAtMyServicePublic() {
    }

    @Around("callAtMyServicePublic()")
    public Object aroundCallAt(ProceedingJoinPoint call) throws Throwable {
        StopWatch clock = new StopWatch(call.toString());
        try {
            clock.start(call.toShortString());
            return call.proceed();
        } finally {
            clock.stop();
            System.out.println(clock.prettyPrint());
        }
    }
}
```

Если запустить вызывающий код с вызовами методов MyService, то получим время вызова каждого метода. Таким образом не меняя вызывающий код и целевой, можно добавить новые функции.

![[Pasted image 20240924133347.png]]


    
**Weaving** — процесс «вплетения» кода advices в нужные места кода приложения.
    
**Target** — метод, чьё поведение изменяется с помощью AOP.

## Как можно реализовать AOP?

- Статически: вплетение на уровне исходников или байт-кода.
- Динамически: создавая прокси и используя вплетение в runtime.
- Spring использует динамический AOP.

# Ресурсы

- [Аспектно-ориентированное программирование](https://habr.com/ru/articles/428548/)
- 

