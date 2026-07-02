---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/sliding-window
difficulty: medium
pattern: "[[Sliding-Window]]"
status: evergreen
related:
  - "[[Two-Pointers]]"
  - "[[HashMap]]"
  - "[[Arrays]]"
  - "[[Strings-DS]]"
  - "[[Prefix-Sum]]"
  - "[[Monotonic-Stack]]"
aliases:
  - Sliding Window
  - Variable window
  - Fixed window
---


# Sliding Window

> [!note] Definition
> Maintain a **window** `[left, right]` over an array/string and slide it to find the optimal subarray/substring satisfying a constraint. The right edge expands, the left edge shrinks — each element enters and leaves at most once → **O(n)**. Two flavors: **fixed size k** and **variable size**.

## When it applies (cues)
- "Longest / shortest contiguous subarray or substring with ..."
- "Maximum sum subarray of size k" (fixed).
- "Longest substring with at most k distinct characters" (variable).
- "Substring with no repeating characters", "min window containing all of T".
- "Count subarrays with sum/product exactly K / at most K".

## Fixed-window template (size k)
```java
int maxSumK(int[] a, int k){
    int sum = 0;
    for (int i = 0; i < k; i++) sum += a[i];
    int max = sum;
    for (int i = k; i < a.length; i++){
        sum += a[i] - a[i-k];        // add new, drop old
        max = Math.max(max, sum);
    }
    return max;
}
```
> [!tip] Fixed window = add the new element, subtract the one that fell out. O(n), O(1).

## Variable-window template (shrink while invalid)
```java
int longest(String s, int k){           // longest with <= k distinct chars
    int[] freq = new int[128]; int left = 0, distinct = 0, best = 0;
    for (int right = 0; right < s.length(); right++){
        if (freq[s.charAt(right)]++ == 0) distinct++;
        while (distinct > k){                 // shrink until valid
            if (freq[s.charAt(left++)]-- == 1) distinct--;
        }
        best = Math.max(best, right - left + 1);
    }
    return best;
}
```
**Invariant**: the window `[left, right]` always satisfies the constraint after the `while` shrinks. Update `best` only when valid.

## The two sub-variants of variable window
1. **Maximize window** (longest valid) → shrink **only until valid**, keep max length.
2. **Minimize window** (shortest valid) → shrink **as much as possible** while valid, record min when valid.
3. **Count subarrays** with property (e.g. sum ≤ k) → use the **atMost(k) − atMost(k−1)** trick (exact = at most − at most).

## Worked: min window substring
```java
String minWindow(String s, String t){
    int[] need = new int[128]; for (char c : t.toCharArray()) need[c]++;
    int have = 0, needCnt = t.length(), l = 0, minLen = Integer.MAX_VALUE, start = 0;
    for (int r = 0; r < s.length(); r++){
        char c = s.charAt(r);
        if (need[c]-- > 0) have++;            // a useful char entered
        while (have == needCnt){              // window is valid -> shrink
            if (r - l + 1 < minLen){ minLen = r-l+1; start = l; }
            if (need[s.charAt(l++)]++ == 0) have--;   // useful char left
        }
    }
    return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start+minLen);
}
```

## Worked: subarray sum equals k (prefix + hashmap, window-adjacent)
> [!tip] Pure sliding window works only for **non-negative** numbers (window sum is monotone). For arrays with negatives, use [[Prefix-Sum]] + [[HashMap]]: count `prefix[r] - k` seen so far. See [[Subarray-Sum-Equals-K]].

## Complexity
- Time: O(n) — each element added once and removed once.
- Space: O(alphabet) for the freq map/array; O(k) if storing window contents.

> [!warning] Pitfalls
  - Using sliding window on arrays with **negative numbers** for sum/product constraints (monotonicity breaks) → use prefix sums instead.
  - Forgetting to shrink (infinite growing window) or shrinking too aggressively (loses valid windows).
  - Updating `best` before the window is valid, or only on shrink — pick the right place based on max vs min variant.
  - Frequency map of a `char` as `int` key — mind `char` to `int` sign; use `c - 'A'` ranges or `int[128]` for ASCII.

## Practice questions
- [[Maximum-Average-Subarray-I]] → fixed window
- [[Longest-Substring-Without-Repeating-Characters]] → variable
- [[Longest-Substring-with-At-Most-K-Distinct-Characters]] → variable
- [[Minimum-Window-Substring]] → variable, shrink-as-much
- [[Fruit-Into-Baskets]] → at most 2 distinct
- [[Permutation-in-String]] → fixed window + freq match
- [[Count-Subarrays-With-Sum-K]] (non-negative) → atMost trick; (with negatives) → prefix sum

## Related
- [[Two-Pointers]] · [[HashMap]] · [[Arrays]] · [[Strings-DS]] · [[Prefix-Sum]] · [[Monotonic-Stack]]
