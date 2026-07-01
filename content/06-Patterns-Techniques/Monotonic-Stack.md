---
type: concept
tags:
  - dsa/pattern/monotonic-stack
difficulty: hard
pattern: "[[Monotonic-Stack]]"
related:
  - "[[Stack]]"
  - "[[Arrays]]"
  - "[[Sliding-Window]]"
aliases:
  - Monotonic Stack
  - Next greater element
  - Stock span
  - Largest rectangle in histogram
---

# Monotonic Stack

> [!note] Definition
> A stack kept in **monotone order** (increasing or decreasing) so that, as you scan, you can answer "next greater/smaller element" queries in **O(n) total**. The invariant: when a new element violates the monotone, pop and **resolve** each popped element using the current one as its answer.

## When it applies (cues)
- "Next greater / next smaller element" (to the right or left).
- "Daily temperatures" (days until a warmer day).
- "Stock span" (consecutive days with price ≤ today).
- "Largest rectangle in histogram" / maximal rectangle in a matrix.
- "Sum of subarray minimums / maximums."
- "Remove k digits to make the smallest number" (monotone increasing stack).
- "Asteroid collision" / "remove adjacent duplicates" (stack with comparison).

## Two flavors
- **Increasing stack** (bottom→top non-decreasing): pops when a smaller element arrives → resolves "next smaller" for the popped.
- **Decreasing stack** (bottom→top non-increasing): pops when a larger element arrives → resolves "next greater" for the popped.
> [!tip] Pick the flavor by the question: "next **greater**" needs a **decreasing** stack (you hold candidates in decreasing order until something bigger arrives). "Next **smaller**" needs an **increasing** stack.

## Template — next greater element (to the right)
```java
int[] nextGreater(int[] a){
    int n = a.length; int[] res = new int[n]; Arrays.fill(res, -1);
    Deque<Integer> st = new ArrayDeque<>();          // indices, values decreasing
    for (int i = 0; i < n; i++){
        while (!st.isEmpty() && a[st.peek()] < a[i])   // current a[i] is the answer for st.peek()
            res[st.pop()] = a[i];
        st.push(i);
    }
    return res;    // remaining indices have no greater element -> -1
}
```
**Store indices, not values** — you need positions to write answers and to compute spans/distances.

## Worked — daily temperatures
```java
int[] dailyTemps(int[] t){
    int n = t.length; int[] res = new int[n];
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = 0; i < n; i++){
        while (!st.isEmpty() && t[st.peek()] < t[i]) res[st.peek()] = i - st.pop();  // distance, not value
        st.push(i);
    }
    return res;
}
```

## Worked — largest rectangle in histogram
```java
int largestRectangle(int[] h){
    int n = h.length, max = 0;
    Deque<Integer> st = new ArrayDeque<>();          // indices, heights increasing
    for (int i = 0; i <= n; i++){
        int cur = i == n ? 0 : h[i];                 // sentinel 0 at the end flushes the stack
        while (!st.isEmpty() && h[st.peek()] > cur){
            int height = h[st.pop()];
            int width = st.isEmpty() ? i : i - st.peek() - 1;   // span between prev-smaller and next-smaller
            max = Math.max(max, height * width);
        }
        st.push(i);
    }
    return max;
}
```
> [!tip] The sentinel (`cur = 0` at index `n`) flushes all remaining bars at the end without duplicating logic. The width formula `i - prevSmaller - 1` uses the stack's new top as the previous-smaller boundary — that's the magic of storing indices.

## Worked — stock span (next greater to the **left**)
```java
int[] span(int[] price){
    int n = price.length; int[] res = new int[n];
    Deque<Integer> st = new ArrayDeque<>();          // indices, prices decreasing
    for (int i = 0; i < n; i++){
        while (!st.isEmpty() && price[st.peek()] <= price[i]) st.pop();
        res[i] = i - (st.isEmpty() ? -1 : st.peek());   // days back to the previous greater
        st.push(i);
    }
    return res;
}
```

## Worked — remove k digits for smallest number
Keep a monotone **increasing** stack of digits; popping a larger left digit for a smaller right one yields a lexicographically smaller number.
```java
String removeK(String num, int k){
    Deque<Character> st = new ArrayDeque<>();
    for (char c : num.toCharArray()){
        while (k > 0 && !st.isEmpty() && st.peek() > c){ st.pop(); k--; }
        st.push(c);
    }
    while (k-- > 0) st.pop();                        // remove from the tail if k remains
    // build result, strip leading zeros
}
```

## Complexity
- Time: O(n) — each element pushed and popped at most once.
- Space: O(n) for the stack.

> [!warning] Pitfalls
  - Storing **values** instead of **indices** when you need positions/spans (histogram, temperatures, stock span).
  - Wrong monotone direction → answers go to the wrong elements. Re-derive: "next greater" = decreasing stack.
  - Forgetting a **sentinel** to flush the stack at the end (histogram, trapping-water-via-stack).
  - Equal elements: decide whether `<=` or `<` triggers a pop (affects "strictly greater" vs "greater-or-equal"); match the problem.
  - Boundary width: `i - st.peek() - 1` assumes the new top is the previous-smaller; if the stack is empty the left boundary is -1 (start of array).

## Practice questions
- [[Next-Greater-Element-I]] / [[Next-Greater-Element-II]] (circular)
- [[Daily-Temperatures]] → days to warmer
- [[Largest-Rectangle-in-Histogram]] → areas
- [[Maximal-Rectangle]] → histogram per row
- [[Stock-Span-Problem]] → next greater left
- [[Sum-of-Subarray-Minimums]] → left/right smaller spans
- [[Remove-K-Digits]] → increasing stack

## Related
- [[Stack]] · [[Arrays]] · [[Sliding-Window]]
