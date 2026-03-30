## Dynamic Programming — 8 patterns
1. **1D DP (Linear)**  
    Fibonacci, house robber.
2. **2D DP (Grid / Table)**  
    LCS, edit distance.
3. **Knapsack-Style DP**  
    0/1, unbounded, subset sum.
4. **DP on Subsequences**  
    LIS, palindromic subsequence.
5. **DP on Strings**  
    Matching, transformations.
6. **DP on Trees**  
    Robber on tree, subtree states.
7. **State Compression (Bitmask DP)**  
    TSP, scheduling.
8. MCM - matrix chain multiplic
9. **Interval DP**  
    Matrix chain, burst balloons.

----
Resources: 
1. [Striver DP](https://www.youtube.com/watch?v=tyB0ztf0DNY&list=PLgUwDviBIf0qUlt5H_kiKYaNSqJ81PMMY&index=2)


Note: Precursor to DP is Recursion. So, master recursion first. 

---
1. Unique Paths II (or Maximal Square) LeetCode 62 with obstacles or 221 (max square of 1s) Standard grid DP.
2. Buy/Sell Stocks 


---
## 1) DP checklist (how to think)

1. **Define state clearly.**
    
    - Ask: “What subproblem am I solving?” (e.g., `dp[i]` = best result using prefix ending at `i`, or `dp[i][j]` = ways to reach cell `(i,j)`).
        
2. **Write recurrence (transition).**
    
    - Express `dp[state]` in terms of smaller states.
        
3. **Base cases.**
    
    - Initialize dp for smallest inputs; handle obstacles/invalid states.
        
4. **Direction & order.**
    
    - For tabulation, pick iteration order that guarantees dependencies computed first.
        
5. **Choose memoization vs tabulation.**
    
    - Prefer tabulation for grid/iterative problems; memoization for complex recursion with branching.
        
6. **Space optimization.**
    
    - Collapse dimensions if you only need previous row/previous k states.
        
7. **Complexity analysis.**
    
    - Time = #states × cost per state. Space = memory for states (and consider optimized).
        
8. **Edge cases & constraints.**
    
    - Zeros, negatives, obstacles, large limits requiring 64-bit, empty input.
        

---

## 2) Common DP patterns you should memorize

- **Linear DP (1D)** — `dp[i]` depends on `dp[i-1]`, `dp[i-2]`, … (Fibonacci, house robber, stock variants)
- **Grid DP (2D)** — `dp[i][j]` depends on neighbours (Unique Paths, Unique Paths II).
- **Knapsack / subset-sum (2D knapsack)** — `dp[i][w]` or 1D optimized.
- **DP with states (hold / not-hold)** — stock problems: maintain `hold` and `cash` states per day.
- **DP over intervals / partitions** — matrix chain, palindrome partitioning.
- **DP + monotonic structures / divide and conquer optimizations** — advanced, keep aware.

---

## 3) Generic DP pseudocode templates (Java-style)

### A. Top-down memoization

```java
Map<State, Result> memo;

Result solve(State s) {
    if (isBase(s)) return baseValue;
    if (memo.containsKey(s)) return memo.get(s);

    Result ans = /* combine solve(smaller states) */;
    memo.put(s, ans);
    return ans;
}
```

### B. Bottom-up tabulation (1D)

```java
int n = ...;
long[] dp = new long[n]; // or appropriate type
dp[0] = base0;
for (int i = 1; i < n; i++) {
    dp[i] = /* compute from dp[i-1], dp[i-2], ... */;
}
return dp[n-1];
```

### C. Bottom-up tabulation (2D)

```java
int R = rows, C = cols;
long[][] dp = new long[R][C];
// init base row/col
for (int i = 0; i < R; i++) {
    for (int j = 0; j < C; j++) {
        dp[i][j] = /* compute using dp[i-1][j], dp[i][j-1], ... */;
    }
}
return dp[R-1][C-1];
```

---

## 4) Problem-specific guidance + pseudocode

### Problem A — **Unique Paths II** (grid with obstacles)

**State:** `dp[i][j]` = number of ways to reach cell `(i,j)` from `(0,0)`.  
**Transition:** if `grid[i][j]` is obstacle → `dp[i][j] = 0`, else `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.  
**Base cases:** `dp[0][0] = (grid[0][0] == free ? 1 : 0)`. First row and column propagate only if no obstacle before them.

```java
int R = grid.length, C = grid[0].length;
long[][] dp = new long[R][C];

for (int i = 0; i < R; i++) {
    for (int j = 0; j < C; j++) {
        if (grid[i][j] == OBSTACLE) {
            dp[i][j] = 0;
            continue;
        }
        if (i == 0 && j == 0) dp[i][j] = 1;
        else {
            long fromTop = (i > 0) ? dp[i-1][j] : 0;
            long fromLeft = (j > 0) ? dp[i][j-1] : 0;
            dp[i][j] = fromTop + fromLeft;
        }
    }
}
return dp[R-1][C-1]; // number of unique paths
```

**Space-opt:** Use a single `long[] row` of size `C` and update in-place:

- `row[j] = (grid[i][j]==OBSTACLE) ? 0 : row[j] + row[j-1]` (with careful order / initializations).
    

**Complexities:** Time `O(R*C)`, Space `O(C)` (optimized) or `O(R*C)`.

---

### Problem B — **Buy and Sell Stocks** (pattern overview + templates)

There are multiple variants. pick one; common ones:

#### Variant 1 — **One transaction (best time I)**

**Idea (greedy):** track `minPrice` so far and `maxProfit = max(maxProfit, price - minPrice)`.

```java
int minPrice = Integer.MAX_VALUE;
int maxProfit = 0;
for (int price : prices) {
    minPrice = Math.min(minPrice, price);
    maxProfit = Math.max(maxProfit, price - minPrice);
}
return maxProfit;
```

#### Variant 2 — **Unlimited transactions (best time II)**

**Idea:** sum of all positive consecutive differences.

```java
int profit = 0;
for (int i = 1; i < n; i++) {
    if (prices[i] > prices[i-1]) profit += prices[i] - prices[i-1];
}
return profit;
```

#### Variant 3 — **At most k transactions (DP)**

**State:** `dp[t][i]` = max profit up to day `i` with at most `t` transactions.  
**Transition:** `dp[t][i] = max(dp[t][i-1], max_{j < i} dp[t-1][j] + prices[i] - prices[j])`.  
Optimize by maintaining `maxDiff = max(maxDiff, dp[t-1][j] - prices[j])`:

```java
int k = ...;
int n = prices.length;
int[][] dp = new int[k+1][n];
for (int t = 1; t <= k; t++) {
    int maxDiff = dp[t-1][0] - prices[0];
    for (int i = 1; i < n; i++) {
        dp[t][i] = Math.max(dp[t][i-1], prices[i] + maxDiff);
        maxDiff = Math.max(maxDiff, dp[t-1][i] - prices[i]);
    }
}
return dp[k][n-1];
```

Space can be optimized to `O(n)` or `O(k)`.

#### Variant 4 — **With cooldown / transaction fee**

Use **state DP** with `hold` and `cash`:

- `hold[i]` = max profit at day `i` when holding a stock.
    
- `cash[i]` = max profit at day `i` when not holding.
    

**Cooldown example transitions:**

```java
hold[i] = Math.max(hold[i-1], cash[i-2] - price[i]);
cash[i] = Math.max(cash[i-1], hold[i-1] + price[i]);
```

(Adjust indices/initialization for cooldown or fee).

---

## 5) How to practice implementing these

1. **Write recurrence and base cases on paper** before coding.
    
2. **Explain invariants** aloud (in interview) — why transition correct, why greedy works (if used).
    
3. **Code first the simplest variant** (e.g., one-transaction or unlimited) then the DP variant.
    
4. **Add space optimizations** after correctness verified.
    
5. **Test edge cases**: empty input, one element, all equal prices, obstacles blocking start/end for grid.
    

---

## 6) Quick complexity summary

- Unique Paths II: `O(R*C)` time, `O(C)` space (optimized).
    
- Stock variants:
    
    - One transaction: `O(n)` time, `O(1)` space.
        
    - Unlimited: `O(n)`, `O(1)`.
        
    - k-transactions: `O(k*n)` time, `O(k*n)` or optimized `O(n)` space.
        
    - Cooldown/fee: `O(n)` time, `O(1)` space with hold/cash states.
        

---
