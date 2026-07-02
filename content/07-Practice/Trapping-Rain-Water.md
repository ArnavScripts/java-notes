---
type: question
tags:
  - dsa/ds/arrays
  - dsa/pattern/prefix-sum
  - dsa/pattern/two-pointers
  - practice
  - practice/hard
difficulty: hard
pattern: "[[Two-Pointers]]"
status: evergreen
source: "LeetCode 42"
related:
  - "[[Two-Pointers]]"
  - "[[Prefix-Sum]]"
  - "[[Monotonic-Stack]]"
aliases:
  - Trapping Rain Water
---


# Trapping Rain Water

> [!note] Problem
> Given an array `height` of non-negative integers representing an elevation map, compute how much water can be trapped after raining.

## Examples
```
Input:  [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

## Brute force
- For each index i, water = `min(maxLeft[0..i], maxRight[i..n-1]) - height[i]`. Precomputing left/right max arrays → O(n) time, O(n) space.

## Optimal approach (one of three)
- Pattern: [[Two-Pointers]] (convergent) — also solvable via [[Prefix-Sum]] max arrays or [[Monotonic-Stack]].
- Idea: water at a position is bounded by the **smaller** of the tallest bars to its left and right. With two pointers from both ends, track `leftMax` and `rightMax`; process the side whose max is smaller (that side's water is fully determined).
- Steps:
  1. `l = 0, r = n-1, leftMax = 0, rightMax = 0, water = 0`.
  2. While `l < r`:
     - if `height[l] < height[r]`: if `height[l] >= leftMax` update `leftMax`; else `water += leftMax - height[l]`; `l++`.
     - else: symmetric with `rightMax` and `r--`.

## Complexity
- Time: O(n) · Space: O(1)

## Java solution (two pointers)
```java
public int trap(int[] height) {
    int l = 0, r = height.length - 1, leftMax = 0, rightMax = 0, water = 0;
    while (l < r) {
        if (height[l] < height[r]) {
            if (height[l] >= leftMax) leftMax = height[l];
            else water += leftMax - height[l];
            l++;
        } else {
            if (height[r] >= rightMax) rightMax = height[r];
            else water += rightMax - height[r];
            r--;
        }
    }
    return water;
}
```

> [!tip] Why processing the smaller side works: if `height[l] < height[r]`, then the water at `l` is bounded by `leftMax` (since the right side already has something taller than `leftMax`'s counterpart). So `l`'s water is decided; advance it. Symmetric for `r`.

## Alternative solutions (same problem, three patterns)
- **Prefix max arrays**: `water = Σ min(L[i], R[i]) - height[i]` — O(n) time, O(n) space. ([[Prefix-Sum]])
- **Monotonic decreasing stack**: pop when a taller bar arrives, water between popped and new top. O(n) time, O(n) space. ([[Monotonic-Stack]])

> [!tip] This is the canonical "one problem, many patterns" exercise — implement all three to deepen intuition.

## Related
- [[Two-Pointers]] · [[Prefix-Sum]] · [[Monotonic-Stack]] · [[Container-With-Most-Water]]
