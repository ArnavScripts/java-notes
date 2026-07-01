---
type: question
tags:
  - practice
  - easy
  - array
  - twopointers
difficulty: easy
pattern: "[[Two-Pointers]]"
source: "LeetCode 167"
related:
  - "[[Two-Pointers]]"
  - "[[Two-Sum]]"
  - "[[Arrays]]"
aliases:
  - Two Sum II
---

# Two Sum II — Input Array Is Sorted

> [!note] Problem
> Given a **1-indexed** sorted-ascending array `numbers` and a `target`, return the 1-based indices of the two numbers adding to `target`. Exactly one solution exists; constant extra space required.

## Examples
```
Input:  numbers = [2,7,11,15], target = 9
Output: [1,2]
```

## Brute force
- Two nested loops or a hash map → O(n²) or O(n) time / O(n) space. But the requirement is O(1) space.

## Optimal approach
- Pattern: [[Two-Pointers]] (convergent)
- Idea: sorted order gives monotonicity — if `a[l]+a[r]` is too small, move `l` right (bigger); too big, move `r` left (smaller).
- Steps:
  1. `l = 0, r = n-1`.
  2. While `l < r`: compare sum to target; move one pointer per step; on equality return `[l+1, r+1]`.

## Complexity
- Time: O(n) · Space: O(1)

## Java solution
```java
public int[] twoSum(int[] numbers, int target) {
    int l = 0, r = numbers.length - 1;
    while (l < r) {
        int sum = numbers[l] + numbers[r];
        if (sum == target) return new int[]{l + 1, r + 1};
        if (sum < target) l++; else r--;
    }
    return new int[]{-1, -1};
}
```

## Related
- [[Two-Pointers]] · [[Two-Sum]] (unsorted, hashmap) · [[3Sum]]
