---
type: question
tags:
  - practice
  - medium
  - string
  - slidingwindow
difficulty: medium
pattern: "[[Sliding-Window]]"
source: "LeetCode 3"
related:
  - "[[Sliding-Window]]"
  - "[[Strings-DS]]"
  - "[[HashMap]]"
aliases:
  - Longest Substring Without Repeating Characters
  - LSWithoutRepeating
---

# Longest Substring Without Repeating Characters

> [!note] Problem
> Given a string `s`, find the length of the **longest substring** without repeating characters.

## Examples
```
Input:  s = "abcabcbb"
Output: 3        // "abc"

Input:  s = "pwwkew"
Output: 3        // "wke"
```

## Brute force
- Check every substring for all-unique chars → O(n³) (or O(n²) with a set per start).

## Optimal approach
- Pattern: [[Sliding-Window]] (variable, shrink while invalid)
- Idea: expand `right`; if `s[right]` already in the window, shrink `left` past the previous occurrence. Track the last index of each char in a map to jump `left` directly.
- Steps:
  1. `Map<Character,Integer> lastPos`.
  2. For `right` in 0..n-1: if seen and `lastPos >= left`, set `left = lastPos + 1`.
  3. Update `lastPos[s[right]] = right`; `best = max(best, right-left+1)`.

## Complexity
- Time: O(n) — each char visited twice (left jumps, doesn't walk one-by-one).
- Space: O(min(n, alphabet)) for the map.

## Java solution
```java
public int lengthOfLongestSubstring(String s) {
    Map<Character,Integer> last = new HashMap<>();
    int left = 0, best = 0;
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (last.containsKey(c) && last.get(c) >= left)
            left = last.get(c) + 1;                       // jump, no slow shrink
        last.put(c, right);
        best = Math.max(best, right - left + 1);
    }
    return best;
}
```

> [!tip] The "jump `left` to `lastPos+1`" form beats the `while` shrink because it's O(1) per character instead of amortized; both are O(n) overall. The `lastPos >= left` guard ensures the duplicate is actually inside the current window.

## Related
- [[Sliding-Window]] · [[Strings-DS]] · [[Longest-Substring-with-At-Most-K-Distinct-Characters]]
