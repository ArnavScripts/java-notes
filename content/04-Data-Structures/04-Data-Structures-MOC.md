---
type: moc
tags:
  - dsa/ds
  - java/ds
  - moc
status: evergreen
related:
  - "[[00-Index-MOC]]"
  - "[[03-Java-Advanced-MOC]]"
  - "[[06-Patterns-MOC]]"
  - "[[05-Algorithms-MOC]]"
aliases:
  - DS MOC
  - Data Structures MOC
---


# Data Structures — MOC

Containers that organize data for efficient operations. Every algorithm runs *on* a data structure; choosing the right one is half the battle.

## Notes
- [[Arrays]] — contiguous, O(1) index
- [[Strings-DS]] — char sequences, immutability, pattern substrings
- [[Linked-List]] — nodes + pointers, O(1) insert/delete at known node
- [[Stack]] — LIFO
- [[Queue]] — FIFO / deque
- [[HashMap]] — key→value, average O(1)
- [[HashSet]] — unique set, average O(1)
- [[BST]] — ordered keys, O(log n) balanced
- [[Heap]] — priority queue, min/max root
- [[Graph]] — vertices + edges, adj list/matrix
- [[Trie]] — prefix tree over strings
- [[Disjoint-Set]] — union-find

> [!tip] Complexity cheat-sheet
> | DS | Access | Search | Insert | Delete | Space |
> |----|:---:|:---:|:---:|:---:|:---:|
> | Array | O(1) | O(n) | O(n) | O(n) | O(n) |
> | Linked List | O(n) | O(n) | O(1)* | O(1)* | O(n) |
> | HashMap | — | O(1)⌀ | O(1)⌀ | O(1)⌀ | O(n) |
> | BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
> | Heap (peek) | O(1) | O(n) | O(log n) | O(log n) | O(n) |
> ⌀ amortized, * at a known node

## Notes in this section
| Note | Difficulty | Status |
| --- | --- | --- |
| [[Arrays]] | easy | evergreen |
| [[BST]] | hard | evergreen |
| [[Disjoint-Set]] | hard | evergreen |
| [[Graph]] | hard | evergreen |
| [[HashMap]] | medium | evergreen |
| [[HashSet]] | easy | evergreen |
| [[Heap]] | medium | evergreen |
| [[Linked-List]] | medium | evergreen |
| [[Queue]] | easy | evergreen |
| [[Stack]] | easy | evergreen |
| [[Strings-DS]] | easy | evergreen |
| [[Trie]] | hard | evergreen |

## Thin notes to expand
| Note | Chars |
| --- | --- |
| [[Stack]] | 2696 |
| [[HashSet]] | 2796 |
| [[Queue]] | 2957 |
| [[Arrays]] | 2962 |
