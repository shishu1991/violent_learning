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