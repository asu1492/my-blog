
![[Pasted image 20240514123344.png]]


Properties of Clean Architecture
1. Independent of Framework 
2. Testable : can be tested without UI, DB, web servers
3. Independent of UI
4. Independent of DB


### Concentric Circle of Software Architecture 

The outermost circle is low level concrete detail. As you move inwards the software grows more abstract, and encapsulates higher level policies. The inner most circle is the most general

**_The Dependency Rule_**
1. name of something declared in an outer circle must not be mentioned by the code in the an inner circle. That includes, functions, classes. variables, or any other named software entity.


Entities 
1. Entities encapsulate _Enterprise wide_ business rules.
2. No operational change to any particular application should affect the entity layer.


Use Cases
1. contains _application specific_ business rules.
2. orchestrate the flow of data to and from the entities,
3. Changes in this layer should not affect entities 
4.  Changes to the externalities such as the database, the UI, or any of the common frameworks should not affect Use Cases. 


Interface Adapter 
1. set of adapters that convert data from the format most convenient for the use cases and entities, to the format most convenient for some external agency such as the Database or the Web
2. No code inward of this circle should know anything at all about the database


Framework and Drivers 
1. composed of frameworks and tools such as the Database, the Web Framework, etc
2. Generally you don’t write much code in this layer other than glue code that communicates to the next circle inwards.
