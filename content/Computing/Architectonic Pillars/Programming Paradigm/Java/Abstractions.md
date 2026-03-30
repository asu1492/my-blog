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



#  Question on Abstraction 
-  What is the purpose of an abstract method?
	- Defining a Common Interface
	- Allowing Partial Implementation
	- Enforcing implementation 
	- Providing a Template for Class Hierarchy
	- Enabling Polymorphism
	- "Program to an Interface, not an Implementation" Principle
- What is abstraction in Java? 
	- Abstraction allow us to define common field and methods that can extended by other classes. 
- How is it achieved?
	- Abstract Class 
	- Abstract Method 
	- Interface 
- What's the difference between an abstract class and an interface?
	- Subclass can only extends one abstract class but it can implement multiple interfaces. 
	- Abstract Class can have both abstract and concrete methods while interfaces use to have only have abstract methods. However since Java 8, interfaces can have:
		- Abstract methods (implicitly public)
		- Default methods (with implementation)
		- Static methods (with implementation)
	- Interface fields explicitly should have keyword 'public static [[final]] while such constraints are not in case of abstract class. 
	- AC can have constructors while interfaces can't. 
	- Interfaces are implicitly public while abstract class can have any access modifier. 
	- AC are bonded at compile time while Interfaces are bonded at runtime(slower)
- Can you have a non-abstract method in an abstract class?
	- Yes, an abstract class can have non-abstract method. 
	- However, if a subclasses extended an abstract class, it should implement the abstract method. 
- Why can't we instantiate an abstract class?
	- Conceptually abstraction
	- Unfinished Design 
	- More of a template 
	- e.g. Prompt Template (is not a prompt and cannot be used to generate a response form LLM)
- Can an abstract class have a constructor?
	- Yes, it can have constructor, however interface cannot have constructor. 
- Can you declare an abstract method static? Why or why not?
	- Abstract methods define a contract that sub class must implement. Hence, they cannot be declared static. 
- How does abstraction differ from encapsulation?
	- Abstraction focuses on what while encapsulation on how. Abstraction operates at design level while encapsulation at implementation level. 
	- Abstraction is achieved through abstract classes and interfaces while encapsulation through access modifiers.
	- Abstraction is about hiding complexity and providing simpler interface while encapsulation is about hiding and controlling access to data. 
	- Encapsulations aids in hiding state of an object by making fields private and providing their access to outer world through public method. 
- Can an abstract class extend another abstract class?
	- Yes, abstract class extends another AC. It is not only possible but a common design practice in OOP. 
- What happens if a class doesn't implement all abstract methods of its abstract superclass?
	- It will give a compile time error
- Can you have private abstract methods? Explain your answer.
	- No we cannot have private abstract method, because by very definition abstract methods are only blueprint and needs to be implemented before it can be instantiated.
	- it will create a compile time contradiction, abstract method need to be implemented by subclass but it cannot be accessed as it is private. 
- How does abstraction help in achieving loose coupling?
	- Abstraction allows creating a blueprint Class and methods that can be implemented by subclass. 
	- It enforces certain constraints about state and functionality while at the same time provide flexibility for sub class to define the actual implementation of functionality.
- What's the difference between abstraction and polymorphism?
	- Abstraction enables for **blueprint** and enforces **contract** that a subclass need to implement. Abstraction is achieved through **abstract classes and interfaces.** 
	- Polymorphism is to **enable different state and functionality.** It is done through method overloading(compile time polymorphism) and method overriding(runtime polymorphism)
- Can you achieve 100% abstraction in Java? If yes, how?
	- 
- How do abstract classes support the "programming to an interface, not an implementation" principle?
- What happens when a subclass does not add annotation "@override" while implementing an abstract method?
	- It will not cause any issue. However we this annotation to let compiler that we are implementing an abstract method of superclass. 
	- If we do not add this annotation, we might end up creating a new method, if there is some typo error in the method signature. 
#corejava
