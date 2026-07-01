---
type: question
tags:
  - practice
  - hard
  - dp
  - string
difficulty: hard
pattern: "[[DP]]"
source: "LeetCode 72"
related:
  - "[[DP]]"
  - "[[Strings-DS]]"
  - "[[Longest-Common-Subsequence]]"
aliases:
  - Edit Distance
  - Levenshtein
---

# Edit Distance

> [!note] Problem
> Given two strings `word1` and `word2`, return the minimum number of operations (insert, delete, replace a character) to convert `word1` into `word2`.

## Examples
```
Input:  word1 = "horse", word2 = "ros"
Output: 3          // horse -> rorse (replace h) -> rose (remove r) -> ros (remove e)

Input:  word1 = "intention", word2 = "execution"
Output: 5
```

## Brute force
- Recurse: at each position try insert/delete/replace → O(3^(m+n)).

## Optimal approach
- Pattern: [[DP]] (2-string, tabulation)
- Idea: `dp[i][j]` = min edits to convert `word1[0..i-1]` to `word2[0..j-1]`. If the last chars match, inherit `dp[i-1][j-1]`; else take 1 + min(insert, delete, replace).
- Recurrence:
  - `word1[i-1] == word2[j-1]`: `dp[i][j] = dp[i-1][j-1]`.
  - else: `dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`
    - `dp[i-1][j]` = delete from word1
    - `dp[i][j-1]` = insert into word1
    - `dp[i-1][j-1]` = replace
- Base: `dp[0][j] = j` (insert j), `dp[i][0] = i` (delete i).

## Complexity
- Time: O(m·n) · Space: O(m·n) (reducible to O(min(m,n)) with two rows).

## Java solution
```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
            else dp[i][j] = 1 + Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]);
        }
    }
    return dp[m][n];
}
```

> [!tip] The "match → diagonal; mismatch → 1 + min of three neighbors" is the canonical 2-string DP shape — shared by [[Longest-Common-Subsequence]] (max instead of 1+min), [[Distinct-Subsequences]] (sum), and [[Regular-Expression-Matching]].

## Space optimization
Keep only the previous row: `dp[j]` updated left-to-right using `prev = dp[i-1][j-1]` saved before overwrite → O(n) space.

## Related
- [[DP]] · [[Strings-DS]] · [[Longest-Common-Subsequence]] · [[Delete-Operation-for-Two-Strings]]
