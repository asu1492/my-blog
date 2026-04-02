1. About Java 
	1. Java is  Pass by Value
2. JAVA overloading vs Java overriding 
	1. OL: class can have methods with same name but needs to have different parameters (number & type). However, you cannot change the return type 
	2. OR: Child class method can override parent class method. 
3. Heap vs Stack Memory 
	1. Stack: dedicated fixed space for prog 
	2. Heap: dynamic memory 
4. Shallow copy vs deep copy 
	5. SC is when you create a new object and ref it to older object. e.g. 
	```
		Example example1 = new Example()
		example1.x = 100;
	
		Example example 2 = example1 
		example2.x=200
	
		System.out.printlN(example1.x) --> this will return 200 
	
	```
1. Garbage Collector: What and How it works? 
	1. https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html 
	2. Free up memory resources and shrinks the heap 
	3. Keep a tab on the objects and its references
	4. ![[Pasted image 20240521211728.png]]
2. Constructor vs Method in a Class? 
	1. C is implicitly called. It is not inherited by child and does not have a return type. Also C is a method but M is not C. 
	2. Method if public in inherited by child. It does have a return type, if it does not return anything we need to add void 
3. Keywords
	1. this: to reference variable in a method defined at a class level. 
	2. super : can be used to invoke method of a parent class in child class when child class is overriding method of a parent class. 
	3. final: How final keyword applied differently across class, method and variables 
		1. create a constant variable that can never be changed 
		2. better practice to create non changing variable with CAPS LOCK 
		3. also if we add final to a method, it cannot be overridden by a child/sub-class 
		4. similarly final class cannot be extended 
4. Class
	1. Abstract Class : When you do not want to create a object of a class and keep it abstract.  
	2. What is a singleton class and how to ensure class is a singleton? 
		1. S is only instance of class exist 
		2. Class is singleton if constructors are private and methods return reference to an instance 
5. Generics: Why used? 
	1. public static List<String> getCodes()
	2. Here String is generics 
6. What is protected/private/public 
	1. Private  : cannot access outside class
	2. protected : cannot access outside a package 
7. Diff b/w equals() and == 
	1. equals() checks the value while == checks memory location 
8. Composition vs Aggregation 
9. Static block 
	1. is used to run logic that we need to run before main method 
10. Data Structure 
	1. Why removal method is faster in linked list than in an array? 
		1. LL we just shift the pointers however in array  we are move each and every object 
	2. ArrayList: How does AL grow dynamically? 
		1. we do not define number of objects, rather internally it increases size once an array is filled. 
11. Comparator vs Comparable : https://www.youtube.com/watch?v=ZA2oNhtNk3w&ab_channel=Telusko 
12. Type Promotion