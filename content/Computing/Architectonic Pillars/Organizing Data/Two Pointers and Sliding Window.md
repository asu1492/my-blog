A fixed window moves both pointers together.  
A variable window moves **r** forward normally, but moves **l** only when a condition is violated (sum > k, too many distinct chars, etc.).

1. [[Container With Most Water]] LeetCode 11 (Medium) Given n non-negative integers a1, a2, ..., an where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines that form a container with the x-axis that contains the most water. Example: Input: [1,8,6,2,5,4,8,3,7] → Output: 49 Approach: Two pointers (left=0, right=n-1), area = min(height[l], height[r]) × (r-l), move the shorter pointer inward. O(n) time, O(1) space
2. [[Longest Substring Without Repeating Characters]] LeetCode 3 (Medium) Given a string s, find the length of the longest substring without repeating characters. Example: "abcabcbb" → 3 ("abc"), "pwwkew" → 3 ("wke") Approach: Sliding window + HashMap/Set to track last index of each char. O(n) time, O(min(m,n)) space (m = charset size)
3. **Remove K Digits** LeetCode 402 (Medium) Given string num representing a non-negative integer and an integer k, return the smallest possible integer after removing k digits. Example: num = "1432219", k = 3 → "1219" Approach: Monotonic increasing stack — pop digits if current is smaller and we have removals left. O(n) time, O(n) space
4. **Valid Palindrome** (or Valid Palindrome II — both asked) LeetCode 125 (Easy) / 680 (Easy) 125: Given a string, ignore non-alphanumeric and case, check if palindrome. 680 variant: At most one character can be deleted. Approach: Two pointers, skip non-alphanumeric, compare lowercase.
5. **Longest Subarray with Sum ≤ K** (ServiceNow favorite variation) Given an array of positive integers and k, find the longest subarray whose sum ≤ k. Example: a = [1,2,3], k=3 → length 2 ([1,2]) Approach: Sliding window — expand right, shrink left when sum > k. O(n) time



----
LC 42: 
![[Pasted image 20251217101139.png]]

# **1. Two-Pointer / Sliding-Window — Questions From _Your Provided List_**

### **A. Pure Two-Pointer**
These use two pointers converging or scanning:
1. Container With Most Water
2. Valid Palindrome
3. S1 = “abcdef”, S2 = “bde” → output “bcde”
4. Min swaps to balance parentheses (two-pointer variant)
5. Search in row+col sorted matrix
6. Next Greater Element (some variants)
7. Remove duplicates from sorted array (I & II)
8. Merge Intervals
9. Intersection of arrays (if solved via pointers)
---
### **B. Pure Sliding Window (Variable window)**

These expand and shrink:
1. Longest Substring Without Repeating
2. Longest Subarray With Sum ≤ k
3. Maximum coins from k consecutive bags (fixed window)
4. Minimum swaps to make string balanced (some solutions)
5. Valid Palindrome (if using preprocessing logic)
6. Longest Palindromic Substring (center expands, but conceptually similar)
7. Jump Game II (greedy window expansion)
---

### **C. Hybrid / Related**
1. Remove K Digits (monotonic pattern)
2. Smallest missing positive (index placement but pointer-like)
3. T9 decoding (stretches pointer logic)

So **14–16 questions from your list** fall under this category.

---

# **2. Questions ServiceNow Commonly Asks in This Pattern**

These are based on repeated patterns from ServiceNow past rounds:

---

### **A. Must-Know (Very Common)**

1. **Minimum Window Substring**
2. **Longest Substring with At Most K Distinct Characters**
3. **Find All Anagrams in a String**
4. **Longest Repeating Character Replacement**
5. **Binary Subarray Sum (fixed + variable windows)**
6. **Sliding Window Maximum** (Deque, but window logic)
7. **Subarrays with sum ≤ k** (variation already in your list)

---

### **B. Moderately Common**

1. **3Sum / 3Sum Closest**
2. **4Sum**
3. **Sort Colors (Dutch National Flag)**
4. **Backspace String Compare**
5. **Two Sum in Sorted Array**
6. **Partition Labels**

---

### **C. Occasionally Asked**
1. **Trapping Rain Water**
2. **Rotate Array using Reversal**
3. **Move Zeroes**
4. **Search in 2D Matrix (top-right pointer)**

---

# **3. How to Use This List in Preparation**

The entire set collapses into **5 patterns you should master**:

### **Pattern 1 — Expand / Shrink window**

- Longest substring without repeating
    
- Min window substring
    
- K distinct characters
    
- Character replacement
    

### **Pattern 2 — Fixed window**

- Sliding window maximum
    
- Maximum coins from k consecutive bags
    
- Find all anagrams
    

### **Pattern 3 — Opposite pointers**

- Container with most water
    
- Valid palindrome
    
- Two-sum in sorted array
    
- Remove duplicates
    

### **Pattern 4 — Partition-based**

- Dutch flag
    
- Move zeroes
    
- Quick-select style pointer shifting
    

### **Pattern 5 — “External” pointer logic**

- Spiral matrix
    
- 2D sorted matrix search
    
- Jump game window expansion
    

---

Here is what you should **remember as templates**, not final solutions.  
These are **patterns**, not answers to any specific question — so they fit study mode.

After each pattern, I’ll ask _one short check-in question_ so you stay active.

---

# ✅ **1. Two-Pointer Template (Java Pseudocode)**

Use this whenever you have _sorted input_, _mirror operations_, or _measure interactions between edges_.

```java
int left = 0;
int right = n - 1;

while (left < right) {

    // ---- Core Logic Here ----
    // compare nums[left], nums[right] 
    // move one pointer based on condition

    if (conditionToMoveLeft) {
        left++;
    } else {
        right--;
    }
}
```

### ✔ When to apply:

- Container With Most Water
    
- Valid Palindrome
    
- Two-sum (sorted)
    
- Trapping Rain Water (two-pointer variant)
    
- Merge operations on sorted input
    

---

### **Check-in:**

In a palindrome check, which pointer typically moves first — left or right?  
(1 sentence)

---

# ✅ **2. Sliding Window (Variable Size) Template**

Used when window grows until a **condition is violated**, then shrinks to fix it.

```java
int left = 0;
Map<Character, Integer> freq = new HashMap<>();

for (int right = 0; right < n; right++) {

    // expand window
    add(nums[right], freq);

    // shrink window when condition fails
    while (windowIsInvalid(freq)) {
        remove(nums[left], freq);
        left++;
    }

    // update answer if needed
    updateAnswer(left, right);
}
```

### ✔ When to apply:

- Longest substring without repeating
    
- Minimum window substring
    
- Longest substring with ≤ K distinct chars
    
- Subarray sum ≤ K
    
- Longest repeating replacement
    

---

### **Check-in:**

In variable-window problems, what triggers the **shrink phase**?

---

# ✅ **3. Sliding Window (Fixed Size k) Template**

Used when the window size **never changes**.

```java
int left = 0;

for (int right = 0; right < n; right++) {

    // expand normally

    if (right - left + 1 == k) {
        // window is full → process it
        updateAnswer(left, right);

        // slide the window
        left++;
    }
}
```

### ✔ When to apply:

- Fixed-size substring/anagram problems
    
- Maximum sum of k elements
    
- Maximum coins in k bags
    

---

### **Check-in:**

What is the fixed window size in “Find all anagrams of a pattern”?

---

# Want the **pattern recognition rules** next?

Example: “How do I instantly know if a question is sliding window vs two-pointer?”