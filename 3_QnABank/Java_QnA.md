# Java Q&A

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