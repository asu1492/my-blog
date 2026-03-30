Resources: 
1. [Binary Search Playlist Striver](https://www.youtube.com/playlist?list=PLF6ChxadzFf8vjafLIxxbKUfarW4V4IOh) 
2. [Greedy Algo Playlist Striver ](https://www.youtube.com/playlist?list=PLgUwDviBIf0rF1w2Koyh78zafB0cz7tea)

- **Search in a Row-Column Sorted 2D Matrix**[ LeetCode 74 ](https://leetcode.com/problems/search-a-2d-matrix/description/)or [240 (Medium)](https://leetcode.com/problems/search-a-2d-matrix-ii/description/) Matrix is sorted row-wise and column-wise. Search for target. Approach: Start from top-right or bottom-left, move like binary search. O(m+n) time
- **Jump Game II** [LeetCode 45 ](https://leetcode.com/problems/jump-game-ii/description/)(Medium) Minimum jumps to reach last index. Approach: Greedy — keep track of current end and farthest reachable. O(n) time
- [**Candy Distribution Problem**](https://www.geeksforgeeks.org/dsa/minimum-number-of-candies-required-to-distribute-among-children-based-on-given-conditions/)  There are A children, B candies of type 1, C of type 2. Each child must get ≥1 candy and no child gets both types. Maximize the minimum candies any child gets (X). Example: A=4, B=4, C=5 → X=2 (2 children get 2 of type1, 2 children get 2 & 3 of type2) Approach: Binary search on X. For a mid=X, children getting type1 ≤ B/X, type2 ≤ C/X, total children covered ≤ A.

---


**Binary-search family**

- Search in rotated sorted array
    
- Find peak element / find first/last position (lower_bound/upper_bound)
    
- Kth smallest in a sorted matrix (binary-search-on-value)
    
- Aggressive cows / partitioning (binary-search-on-answer)
    
- Sqrt / integer division boundary problems
    

**Greedy family**

- Jump Game I (feasibility)
    
- Interval scheduling / activity selection / minimum platforms / meeting rooms
    
- Assign cookies / maximize tasks done with deadlines
    
- Gas station (circular greedy)
    
- Fractional knapsack (greedy baseline vs DP knapsack)
    

**Combined / related**

- Sliding-window + binary search (smallest subarray with sum ≥ S)
    
- Two-pointer variants on sorted arrays (3Sum, 4Sum)
    
- Heaps + greedy (top-K, merge-K lists)
    
- Monotonic deque (sliding window max)
    

---

## Key skills to practice for each problem

1. Identify pattern (binary search on index vs on answer).
    
2. Write clear invariants (what condition holds before/after loop).
    
3. Prove correctness briefly (why mid moves left/right).
    
4. Analyze time/space.
    
5. Implement cleanly and test edge cases (empty, single element, duplicates, extremes).
    
6. Optimize constant factors and explain trade-offs.
    

---

## Two compact templates (pseudocode) to keep in mind

**A. Binary search on answer (find max X such that feasible(X))**

```
int lo = lowBound;   // problem-specific
int hi = highBound;  // problem-specific

while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;
    if (feasible(mid)) {
        lo = mid + 1;   // mid works, try larger
    } else {
        hi = mid - 1;   // mid fails, reduce
    }
}
return hi; // largest feasible


```
**B. Greedy interval sweep (maximize non-overlapping intervals)**

```
sort(intervals by endTime);
int count = 0;
int lastEnd = -infty;

for (interval : intervals) {
    if (interval.start > lastEnd) {
        count++;
        lastEnd = interval.end;
    }
}
return count;

```
---

## Recommended practice set (10–12 problems to cover breadth)

- Search in 2D matrix (74 / 240) — your list
    
- Search in rotated sorted array
    
- Kth smallest in sorted matrix
    
- Smallest subarray with sum ≥ S (binary-search-on-window or two-pointer)
    
- Jump Game I & II
    
- Minimum number of platforms / Meeting rooms (heap)
    
- Aggressive cows / Partition array into k subarrays (binary-search-on-answer)
    
- Interval scheduling / Merge intervals
    
- Assign cookies / Gas station
    
- Sliding window maximum (deque)
    
- K pairs with smallest sums (heap + greedy)
    

Solve: implement, test, explain complexity, and state invariants.
