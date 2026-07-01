---
type: concept
tags:
  - dsa/pattern/reversal
difficulty: medium
pattern: "[[In-Place-Reversal]]"
related:
  - "[[Linked-List]]"
  - "[[Arrays]]"
  - "[[Two-Pointers]]"
aliases:
  - In-Place Reversal
  - Reverse linked list
  - Reverse in groups
---

# In-Place Reversal

> [!note] Definition
> Reverse a contiguous run in place by swapping pointers (linked list) or values (array), using O(1) extra space. The building block: reverse-the-whole; composed with offsets it yields "reverse a sublist", "reverse in groups of k", "rotate an array".

## When it applies (cues)
- "Reverse a linked list / a portion [m, n]."
- "Reverse every k nodes in a list."
- "Rotate an array / list by k."
- "Reverse words in a string" (reverse all, then reverse each word).
- "Reverse a subarray as part of a bigger transform."

## Template — reverse a singly linked list
```java
ListNode reverse(ListNode head){
    ListNode prev = null, cur = head;
    while (cur != null){
        ListNode next = cur.next;   // save
        cur.next = prev;            // flip
        prev = cur; cur = next;     // advance
    }
    return prev;                    // new head
}
```
> [!tip] Save `next` **before** overwriting `cur.next`, else you lose the rest of the list.

## Template — reverse a sublist [m, n] (1-based)
```java
ListNode reverseBetween(ListNode head, int m, int n){
    ListNode dummy = new ListNode(0); dummy.next = head;
    ListNode pre = dummy;
    for (int i = 1; i < m; i++) pre = pre.next;          // node before the run
    ListNode cur = pre.next, then = cur.next;
    for (int i = 0; i < n - m; i++){                     // insert 'then' after 'pre' one at a time
        cur.next = then.next;
        then.next = pre.next;
        pre.next = then;
        then = cur.next;
    }
    return dummy.next;
}
```
The "head-insertion" trick reverses in place without a separate reverse step — each iteration moves `then` to the front of the run.

## Template — reverse in groups of k
```java
ListNode reverseK(ListNode head, int k){
    ListNode dummy = new ListNode(0); dummy.next = head;
    ListNode pre = dummy;
    while (true){
        ListNode kth = pre;
        for (int i = 0; i < k && kth != null; i++) kth = kth.next;   // is there a full group?
        if (kth == null) break;
        ListNode cur = pre.next, then = cur.next;
        for (int i = 0; i < k-1; i++){                                // reverse k-1 links
            cur.next = then.next; then.next = pre.next; pre.next = then; then = cur.next;
        }
        pre = cur;                                                    // next group's pre
    }
    return dummy.next;
}
```

## Template — rotate array by k (reverse method)
```java
void rotate(int[] a, int k){
    int n = a.length; k %= n;                 // k may exceed n
    reverse(a, 0, n-1);                       // whole
    reverse(a, 0, k-1);                       // first k
    reverse(a, k, n-1);                       // rest
}
void reverse(int[] a, int l, int r){ while (l < r){ int t=a[l]; a[l++]=a[r]; a[r--]=t; } }
```
> [!tip] "Rotate right by k" = reverse-all, reverse-first-k, reverse-rest. Three reverses, O(n) time, O(1) space — beats the O(n)-space copy approach.

## Template — reverse words in a string
```java
String reverseWords(String s){
    char[] c = s.trim().toCharArray();
    reverse(c, 0, c.length-1);                       // reverse whole
    int i = 0;                                       // reverse each word + collapse spaces
    StringBuilder out = new StringBuilder();
    while (i < c.length){
        while (i < c.length && c[i] == ' ') i++;     // skip spaces
        int j = i;
        while (j < c.length && c[j] != ' ') j++;     // word end
        reverse(c, i, j-1);
        if (i < j) { if (out.length()>0) out.append(' '); out.append(c, i, j-i); }
        i = j;
    }
    return out.toString();
}
```

## Complexity
- Time: O(n) — every link/index touched O(1) times.
- Space: O(1) — a few pointers/variables.

> [!warning] Pitfalls
  - Forgetting the **dummy head** for sublist/group reversals → head itself may move; without dummy you must special-case it.
  - Losing the tail linkage → the node after the reversed run must be reconnected (`cur.next = then` after the loop).
  - Recursion-based reverse on huge lists → stack overflow; prefer iteration ([[JVM-and-Memory]]).
  - In `rotate`, normalize `k %= n` first; negative k or k ≥ n otherwise breaks.
  - Reverse-words: multiple spaces / leading-trailing spaces — collapse explicitly.

## Practice questions
- [[Reverse-Linked-List]] → whole
- [[Reverse-Linked-List-II]] → sublist
- [[Reverse-Nodes-in-k-Group]] → groups of k
- [[Rotate-Array]] → three reverses
- [[Reverse-Words-in-a-String]] → reverse + per-word
- [[Palindrome-Linked-List]] → reverse second half

## Related
- [[Linked-List]] · [[Arrays]] · [[Two-Pointers]]
