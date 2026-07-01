---
type: question
tags:
  - practice
  - hard
  - stack
  - monotonicstack
difficulty: hard
pattern: "[[Monotonic-Stack]]"
source: "LeetCode 84"
related:
  - "[[Monotonic-Stack]]"
  - "[[Stack]]"
  - "[[Maximal-Rectangle]]"
aliases:
  - Largest Rectangle in Histogram
---

# Largest Rectangle in Histogram

> [!note] Problem
> Given an array `heights` of non-negative integers (bar heights of width 1), find the area of the **largest rectangle** that can be formed in the histogram.

## Examples
```
Input:  heights = [2,1,5,6,2,3]
Output: 10          // the 5x2 rectangle (bars 5 and 6)
```

## Brute force
- For each bar, expand left and right while bars are ≥ its height → O(n²).

## Optimal approach
- Pattern: [[Monotonic-Stack]] (increasing stack of indices)
- Idea: a bar's maximal rectangle uses it as the shortest bar; its width extends from the **previous smaller** to the **next smaller** index. Maintain an increasing stack; when a smaller bar arrives, pop taller bars and compute their area using the current index as the right boundary and the new stack top as the left boundary.
- Steps:
  1. Iterate `i` from 0..n inclusive (treat index n as height 0 to flush).
  2. While the stack's bar > current: pop it; height = `heights[popped]`; width = `stack.isEmpty() ? i : i - stack.peek() - 1`; update max area.
  3. Push `i`.

## Complexity
- Time: O(n) — each index pushed/popped once.
- Space: O(n) for the stack.

## Java solution
```java
public int largestRectangleArea(int[] heights) {
    int n = heights.length, max = 0;
    Deque<Integer> st = new ArrayDeque<>();           // indices, heights increasing
    for (int i = 0; i <= n; i++) {
        int cur = i == n ? 0 : heights[i];            // sentinel flushes stack
        while (!st.isEmpty() && heights[st.peek()] > cur) {
            int h = heights[st.pop()];
            int w = st.isEmpty() ? i : i - st.peek() - 1;
            max = Math.max(max, h * w);
        }
        st.push(i);
    }
    return max;
}
```

> [!tip] The sentinel (`cur = 0` at `i = n`) flushes the remaining bars at the end with identical logic — no duplicate code. The width formula `i - st.peek() - 1` uses the **new top** after popping as the previous-smaller boundary; if the stack is empty the left boundary is -1 (array start), so width = `i`.

## Extension
- [[Maximal-Rectangle]] — treat each matrix row as a histogram baseline and apply this per row.

## Related
- [[Monotonic-Stack]] · [[Stack]] · [[Maximal-Rectangle]] · [[Sum-of-Subarray-Minimums]]
