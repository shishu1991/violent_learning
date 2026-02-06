| Q | Ans |
|---|-----|
| JDK vs JRE vs JVM | JVM runs bytecode; JRE = JVM + libs; JDK = JRE + dev tools. |
| OOP Pillars | Encapsulation, Inheritance, Polymorphism, Abstraction. |
| String immutability | Security, thread-safety, pooling, stable hashing. |
| ArrayList vs LinkedList | ArrayList: random access fast; LinkedList: mid inserts/removes better. |
| equals/hashCode contract | Equal objects must have equal hash codes; required for HashMap/HashSet. |
| final, finally, finalize() | final: cannot change/override; finally: always runs; finalize(): deprecated GC hook. |
| Autoboxing/Unboxing | Auto convert between primitives and wrappers; watch NPE and overhead. |
| static keyword | Belongs to class; shared; static methods can't access instance members directly. |
| throw vs throws | throw: actually throws exception; throws: declares possible exceptions. |
| String pool vs new String | Literals go to pool; new creates distinct heap object. |
| Integer == cache | -128..127 cached; use equals() for wrappers. |
| Can static be overridden? | No—static methods are hidden, not overridden (compile-time binding). |
| Is HashMap thread-safe? | No; use ConcurrentHashMap for concurrency. |
| Interface variables | Implicitly public static final; no instance fields. |
| wait() in while | Guard against spurious wakeups; re-check condition. |
| equals without hashCode | Breaks hash collections; may not find elements. |
| volatile and ++ | volatile gives visibility, not atomicity; use AtomicInteger or locks. |
| finally suppressing try exception | Exception/return in finally overrides try's exception. |
| Why Map not Collection | Map handles key-value pairs; contract differs from Collection of elements. |

1. What is JDK JRE and JVM and difference?
> JVM is the core where the byte code __(.java file converted to byte code by Java Compiler and interpretor)__ runs and program is executed. JRE holds the JVM + Java Library to run and compile the .java file while JDK helps you to create application in java and various (java or third party lib) + dev tools.


2. OOPs concept and why object programing?
> Java is one of the language that implements the OOPs concepts at its core. OOPs help use to manage the programs by providing following things with it.

- __Class__ is the blueprint of an object. (Fields and methods)
- __Object__ is nothing but an instance of the class loaded in memory (state and behaviour)
- __Encapsulation__ is the concept of hiding the state/fields of the object and allowing the modification of the states/fields via calling behaviour/method
- __Plymorphism__ It is the concept of when a method with same name but have different behavior or forms. It is of 2 types 
    - Overerloading or compiletime  : same name but different signature & behavior
    - overriding | runtime | dynamic binding : same name and signature but behaviour is different | uses inheritance in action 
- __Inheritance__ It is the cocept of extending the class functionality using existing class. (Base/Parent/General and Child/Specific)
- __Abstraction__ It is the concept of Hidng or abstracting the implementation details using (Interfaces & abstract classes)
    - Interfaces : have no implementaion details. Classes which implements the interface have to provide the implementation to all methods. Object cannot be created from interfaces. it can store references.

    - Abstract classes : have at least one method which has no implentation.
    They have the constructor but cannot be initialized as object need class to be implement the abstract method. It can store reference 

3. What is immutability and how to create an immutable object with example?

> It is the object intialization process that creates an object with final field values (such that cannot be updated or modifed after object intialization).

> It is used for thread safety and security purpose. 

> To have class immutable we have make th class final fields private final initialize the field via constructor. No setter method for fields. return defensive copy of the fields.

> Example String INteger Bigdecimal | date time Key in hashmap.

> Use stringbuilder where non immutable object is required.

4. Object class and its method?

> It is the default implemented class in jave which is extended by every class 
> Important methods
- toString: returns object representation of object
- equals: used to compare content | default is reference compariosion | 
- hascode : return hash of the object | must be consitent with equals 
- getClass(): return runtime class info
- clone () : returns clone of the object | shallow copy
- wait() | notify() | notifyAll()  


