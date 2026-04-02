

1. The interpreter reads a high-level program and executes it
2. In contrast, a compiler reads the entire program(source code) and translates it(object code or executable) completely before the program starts running
3. Source code is portable while object code is not. 

Java addresses the issue of compiler and interpreter by having the **compiler(javac) return byte code** that is run across various systems using JVM(Interpreter)