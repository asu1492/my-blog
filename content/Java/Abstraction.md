---
title: Abstraction in Java
date: 2025-03-27
tags:
  - java
  - abstraction
  - corejava
---

## Questions on Abstraction

### What is the purpose of an abstract method?
- Defining a Common Interface
- Allowing Partial Implementation
- Enforcing Implementation
- Providing a Template for Class Hierarchy
- Enabling Polymorphism
- "Program to an Interface, not an Implementation" Principle

### What is abstraction in Java?
Abstraction allows us to define common fields and methods that can be extended by other classes.

### How is it achieved?
- Abstract Class
- Abstract Method
- Interface

### What's the difference between an abstract class and an interface?
- A subclass can only extend **one** abstract class but can implement **multiple** interfaces.
- Abstract classes can have both abstract and concrete methods. Interfaces historically only had abstract methods, but since **Java 8** interfaces can also have:
  - Abstract methods (implicitly public)
  - Default methods (with implementation)
  - Static methods (with implementation)
- Interface fields must explicitly be `public static final`, while abstract class fields have no such constraint.
- Abstract classes can have constructors; interfaces cannot.
- Interfaces are implicitly public; abstract classes can have any access modifier.
- Abstract classes are bound at compile time; interfaces are bound at runtime (slightly slower).

### Can you have a non-abstract method in an abstract class?
Yes. An abstract class can have non-abstract (concrete) methods. However, if a subclass extends an abstract class, it must implement all abstract methods.

### Why can't we instantiate an abstract class?
- Conceptually it represents abstraction — it is unfinished by design.
- It is more of a template (e.g., a Prompt Template is not itself a prompt and cannot generate a response from an LLM).

### Can an abstract class have a constructor?
Yes — unlike interfaces, abstract classes can have constructors (used by subclass constructors via `super()`).

### Can you declare an abstract method `static`? Why or why not?
No. Abstract methods define a contract that subclasses must implement through dynamic dispatch. `static` methods belong to the class itself and cannot be overridden, which contradicts the purpose of abstract methods.

### How does abstraction differ from encapsulation?
| | Abstraction | Encapsulation |
|---|---|---|
| Focus | *What* an object does | *How* it does it |
| Level | Design level | Implementation level |
| Achieved via | Abstract classes & interfaces | Access modifiers |
| Goal | Hide complexity, provide simpler interface | Hide & control access to data |

### Can an abstract class extend another abstract class?
Yes — and it is a common design practice in OOP. The intermediate abstract class does not need to implement the parent's abstract methods; the first concrete subclass must.

### What happens if a class doesn't implement all abstract methods of its abstract superclass?
A **compile-time error** occurs. The subclass must either implement all abstract methods or itself be declared abstract.

### Can you have private abstract methods?
No. Abstract methods must be overridden by subclasses, so they must be at least `protected`. A `private abstract` method creates a compile-time contradiction — subclasses can't access it to implement it.

### How does abstraction help in achieving loose coupling?
Abstraction allows creating a blueprint (class/interface) that subclasses implement. It enforces constraints on state and functionality while giving subclasses flexibility for their actual implementations — callers depend on the abstraction, not any specific implementation.

### What's the difference between abstraction and polymorphism?
- **Abstraction** provides a blueprint and enforces a contract that subclasses implement. Achieved through abstract classes and interfaces.
- **Polymorphism** enables different behavior at runtime. Achieved through method overloading (compile-time) and method overriding (runtime).

### Can you achieve 100% abstraction in Java?
Yes — through **interfaces**. A pure interface with only abstract method signatures has 100% abstraction (before Java 8 default methods).

### What happens when a subclass omits `@Override` while implementing an abstract method?
No compile error occurs. However, `@Override` is recommended — it lets the compiler verify you're correctly implementing the superclass contract. Without it, a typo in the method signature would silently create a new method instead of implementing the abstract one.
