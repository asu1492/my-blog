
## References
1. https://docs.oracle.com/javase/7/docs/api/java/lang/String.html
2. String Builder - Supported Method - https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html


## Notes
1. String - immutable - string constant pool 
2. == compares references while equals() compares content 
3. Mutable via String Buffer and String Builder, former is thread-safe as it is synchronous while later is faster.
4. a = "Abhishek"; b = "Abhishek" --> this does not create new string references the same from string constant pool. However, String a = new String("Abhishek), String b = new String("Abhishek) - this creates two string in string constant pool.  
5. String implements following Interfaces 
	1. [[Serializable]]
	2. Comparable 
	3. CharSeq
6. Using `Optional<String>` instead of directly using `String` has several advantages:
	1. **Avoiding NullPointerExceptions**: By using `Optional`, you can avoid `NullPointerExceptions` that commonly occur when dealing with `null` values. It enforces explicit handling of the possibility of absence.
	2. **Expressiveness**: `Optional` makes your code more expressive by clearly indicating that a value may or may not be present. This improves code readability and helps convey the intention of the code to other developers.
	3. **Forced Handling**: When you have an `Optional<String>`, you're forced to handle both cases where a value is present and where it's absent. This encourages you to write more robust and defensive code.
	4. **API Design**: `Optional` is part of the Java API, and many methods in the standard library return `Optional` to represent the potential absence of a value. Using `Optional` in your own code aligns with this convention and makes your API more consistent.
	5. **Encourages Functional Programming**: `Optional` encourages a more functional programming style by providing methods like `map`, `filter`, and `flatMap`, which allow you to perform operations on the wrapped value without directly accessing it.
	6. However, it's essential to use `Optional` judiciously. It's <font style="color:red">not suitable for every situation, especially when dealing with collections or methods that already return `null`</font>. Overuse of `Optional` can lead to verbosity and decreased readability. It's generally recommended to use `Optional` for return types in public APIs or in situations where the absence of a value is meaningful and needs to be explicitly handled.
7. 



## Code 
### Palindrome
```
 String cleanStr = str.replaceAll("[^a-zA-Z0-9]", "").toLowerCase();
 
 // Initialize pointers at the beginning and end of the string
  int left = 0;
  int right = cleanStr.length() - 1;
   
// Compare characters from both ends towards the center
   while (left < right) {
        if (cleanStr.charAt(left) != cleanStr.charAt(right)) {
                return false; // If characters don't match, it's not a palindrome
            }
        left++;
        right--;
    } 
   } 
```