```java 
/**
     * Brute-force / quadratic approach based on the handwritten notes:
     * For each start index i, scan j = i..n-1 and keep a boolean[256] seen table.
     * Stop the inner loop when a repeated char is found.
     *
     * Assumption: input uses ASCII (256). For full Unicode use a Set<Character>.
     *
     * Time: O(n^2) worst-case
     * Space: O(1) (boolean[256] per outer iteration)
     */
    public static int lengthOfLongestSubstringBrute(String s) {
        if (s == null || s.length() == 0) return 0;

        int n = s.length();
        int maxLen = 0;

        for (int i = 0; i < n; i++) {
            boolean[] seen = new boolean[256]; // reset for each start i
            for (int j = i; j < n; j++) {
                char c = s.charAt(j);
                if (seen[c]) {
                    // repeated character -> this substring [i..j] is invalid; stop extending
                    break;
                }
                seen[c] = true;
                int len = j - i + 1;
                if (len > maxLen) maxLen = len;
            }
            // optional small optimization: if remaining length <= maxLen, break early
            if (n - i <= maxLen) break;
        }

        return maxLen;
    }
```

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        if (n == 0) return 0;

        int[] last = new int[256];
        java.util.Arrays.fill(last, -1);

        int left = 0;           // window start
        int right = 0;          // window end
        int maxLen = 0;

        while (right < n) {
            char c = s.charAt(right);

            // if character was seen inside the current window
            if (last[c] >= left) {
                left = last[c] + 1;      // shrink window from the left
            }

            // update window info
            int len = right - left + 1;
            maxLen = Math.max(maxLen,len);
            last[c] = right;             // update last occurrence of char c
            right++;                     // expand window to the right
        }

        return maxLen;
    }
}
```

