---
type: question
tags:
  - dsa/ds/stack
  - dsa/ds/strings
  - dsa/pattern/stack
  - practice
  - practice/easy
difficulty: easy
pattern: "[[Stack]]"
status: evergreen
source: "LeetCode 20"
related:
  - "[[Stack]]"
  - "[[Strings-DS]]"
aliases:
  - Valid Parentheses
---


# Valid Parentheses

> [!note] Problem
> Given a string of `()[]{}`, determine if it is valid: every open bracket is closed by the same type in the correct order.

## Examples
```
Input:  "()[]{}"        Output: true
Input:  "([)]"          Output: false
Input:  "{[]}"          Output: true
```

## Brute force
- Repeatedly remove adjacent matching pairs `"()"`, `"[]"`, `"{}"` until no change. O(n²) worst case.

## Optimal approach
- Pattern: [[Stack]]
- Idea: push openers; on a closer, check the top matches and pop; at the end the stack must be empty.
- Steps:
  1. For each char: if opener, push; if closer, the stack must be non-empty and top must be the matching opener, else invalid.
  2. Valid iff stack empty at the end.

## Complexity
- Time: O(n) · Space: O(n)

## Java solution
```java
public boolean isValid(String s) {
    Deque<Character> st = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(') st.push(')');
        else if (c == '[') st.push(']');
        else if (c == '{') st.push('}');
        else {                          // c is a closer
            if (st.isEmpty() || st.pop() != c) return false;
        }
    }
    return st.isEmpty();
}
```

> [!tip] Push the **expected closer** instead of the opener — then validation is just `st.pop() != c`, no matching table needed.

## Related
- [[Stack]] · [[Strings-DS]] · [[Min-Stack]]
