---
title: Arrays
date: 2025-03-28
tags:
  - dsa
  - arrays
  - leetcode
---

## Coding Problems

### Two Sum

> Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

**Brute Force — O(n²) — ~45ms**

```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{};
}
```

**Optimised with HashMap — O(n) — ~2ms**

The key insight: for each element `nums[i]`, we need to know if its complement (`target - nums[i]`) has already been seen.

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{ map.get(complement), i };
        }
        map.put(nums[i], i);
    }

    throw new IllegalArgumentException("No two sum solution found");
}
```

---

### Best Time to Buy and Sell Stock

> Given an array `prices` where `prices[i]` is the price of a stock on day `i`, return the maximum profit from a single buy-sell transaction. Return `0` if no profit is possible.

**Brute Force — O(n²)**

```java
public int maxProfit(int[] prices) {
    int maxProfit = 0;
    for (int i = 1; i < prices.length; i++) {
        for (int j = 0; j < i; j++) {
            int profit = prices[prices.length - 1 - j] - prices[i - 1 - j];
            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }
    }
    return maxProfit;
}
```


**Patterns**
-  [[Two Pointers and Sliding Window]]
	- Opposite ends or same direction; used for sorted arrays, partitioning, palindromes.
	- Fixed or variable window for subarray/substring problems.
- **Prefix Sum / Difference Array**  
    Range queries, cumulative counts, subarray sums.
- **Binary Search (on index or answer)**  
    Sorted arrays or monotonic condition.
- **Greedy / In-place Transformation**  
    Local optimal choices, rearrangement, marking in array itself.