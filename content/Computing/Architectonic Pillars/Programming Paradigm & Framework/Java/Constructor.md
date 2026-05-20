# Constructor

**Summary**: Covers Constructor in Java, including the core concept, practical usage, and interview-relevant points.
**Tags**: #programming #java #constructor
**Created**: Unknown
**Last Updated**: 2026-05-20

---

1. Why Constructor 
	1. A constructor is a special method in Java used to initialize an object’s state (instance variables). Unlike regular methods, it does not have a return type (not even `void`) and is automatically called when an object is created using the `new` keyword.. 
2. How 
	1. Default - If no constructor is defined, and when we create a new object using new keyword, default constructor called by java. 
	2. Parameterised - Once you have parameterised constructor and you want to use a no agrs constructor, we will have to create one, compiler will not provide default constructor. 
	3. Overloading - multiple constructor 
3. What is constructor chaining? 
	1. to call constructor from within the constructor 
	2. this keyword is used to refer to the constructor in the same class while super() is used to refer to the constructor in the parent class. 
	3. super() is necessary to initialize parameterized constructor of parent class. 


![[Pasted image 20250201141214.png]]

---

## Related Notes

- [[00. Master Knowledge Map]]
- [[Programming Paradigm & Framework/00. Overview|Programming Overview]]
- [[Programming Paradigm & Framework/Java/0. Java Roadmap|Java Roadmap]]
