
## Stacks & Queues — 3 patterns
- **Monotonic Structure**  
    Maintain increasing/decreasing order.  
    Examples: next greater/smaller, remove k digits, sliding window max.
- **Expression Parsing**  
    Operator precedence, parentheses, evaluation.  
    Examples: basic calculator, RPN, infix → postfix.
- **State Tracking**  
    Stack as memory of previous state.  
    Examples: remove adjacent duplicates, decode string, simplify path.
- **Level / Order Processing**  
    Queue-based stepwise processing.  
    Examples: BFS, rotting oranges, multi-source propagation.

----
1. **Next Greater Element to the Right** Given array, for each element find the next greater element on its right (-1 if none). Example: [11,13,21,3] → [13,21,-1,-1] Approach: Monotonic decreasing stack from right to left. O(n) time, O(n) space
2. **Valid Parentheses** LeetCode 20 (Easy) Standard stack problem with (), {}, [].
3. **Minimum Swaps to Make String Balanced** LeetCode 1963 (Medium) You are given a string s of '[' and ']'. In one swap you can swap any two characters. Return minimum swaps to make it balanced. Approach: Track imbalance count, every time count goes negative → need a swap.
4. **Remove All Adjacent Duplicates in String** (or II with k=2) [LeetCode 1047](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/) / [1209]( https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/description/) Use stack to remove "aa" pairs repeatedly.
5. [**Basic Calculator II** LeetCode 227](https://leetcode.com/problems/basic-calculator-ii/description/) (Medium) Implement calculator with +,−,*,/ (no parentheses). Approach: Stack — push numbers, when see * or / compute immediately, at end process +− from left to right.


---
   42.⁠ ⁠Trapping Rain Water
496. Next Greater Element I

497. Next Greater Element II

498. Next Greater Node In Linked List

499. Next Greater Element IV

500. Number of Visible People in a Queue

501. Online Stock Span

84.⁠ ⁠Largest Rectangle in Histogram

1856. Maximum Subarray Min-Product

-----



# **1. High-Priority Stack Problems (these appear the most in ServiceNow rounds)**

### **A. Next Greater / Smaller Element Family**

This entire family is _very frequently asked_:

- Next Greater Element (left / right)
    
- Next Smaller Element
    
- Daily Temperatures
    
- Stock Span Problem
    
- Nearest Smaller to Left (already in your list)
    

**Pattern:**  
Monotonic stack (increasing or decreasing).

---

### **B. Expression Evaluation**

Common in ServiceNow system-design + JS rounds:

- Basic Calculator I & II
- Reverse Polish Notation
- Valid Parentheses
- Minimum swaps to balance parentheses (stack variant)
- Remove Adjacent Duplicates using Stack

**Pattern:**  
Operators, precedence, stack for numbers & operators.

---

### **C. Parentheses / Bracket Problems**

- Generate Parentheses
    
- Validate parentheses
    
- Balance parentheses
    
- Minimum swaps to balance
    
- Longest valid parentheses
    

**Pattern:**  
Use stack or counter depending on constraints.

---

### **D. String Reduction / Cleanup Using Stack**

Often asked in easy-medium questions:
- Remove K Digits (monotonic decreasing stack)
- Remove All Adjacent Duplicates :
	- 
	-
- Remove Adjacent Duplicates II (with counts)
- Backspace String Compare (stack version)


**Pattern:**  
Build final string via stack operations.

---

### **E. Stack for Data Structure Design**

- Implement Min Stack
    
- Implement Max Stack
    
- Stack using Queues
    
- Queue using Stacks
    

**Pattern:**  
Understand cost differences (push vs pop heavy).

---

### **F. Tree Problems Using Stack**

- Iterative DFS
    
- Inorder / Preorder / Postorder traversal (iterative)
    

**Pattern:**  
Simulate recursion.

---

---

# **2. High-Priority Queue Problems**

### **A. BFS / Level Order**

- Rotten Oranges
    
- Course Schedule
    
- Shortest path in grid
    
- Number of rooms/meeting rooms
    

**Pattern:**  
Queue for level-based processing.

---

### **B. Sliding Window with Deque**

- Sliding Window Maximum
    
- First negative number in window
    
- Maximum of subarrays of size k
    

**Pattern:**  
Monotonic deque.

---

### **C. Priority Queue (Heap) Problems**

These are technically queues:

- K largest elements
    
- Meeting rooms (min-heap)
    
- Top K frequent elements
    
- Merge K sorted lists
    

**Pattern:**  
Min-heap or max-heap for ordering.

---

### **D. Classic Queue Design / Operations**

- Implement LRU Cache (Queue + HashMap)
    
- Hit counter / rate limiter (queue sliding log)
    
- Producer–consumer (BlockingQueue concept)
    

---

# **3. What You Should Prioritize (Based on Your JD & Past Feedback)**

### **Priority Level 1 (Must Know)**

- Monotonic Stack
    
- Expression evaluation
    
- Sliding window maximum (deque)
    
- Next Greater Element (all variants)
    
- Remove K Digits
    
- Parentheses problems
    
- BFS (queue) fundamentals
    

### **Priority Level 2 (Strong to Have)**

- LRU Cache
    
- Min Stack
    
- Stack/Queue implementation questions
    
- Backspace String Compare
    

### **Priority Level 3 (Optional)**

- Iterative tree traversals
    
- Queue-based rate limiter
    

This prioritization is enough for ServiceNow’s typical bar.

    

---
