---
type: moc
tags:
  - practice
  - pattern-map
status: evergreen
related:
  - "[[07-Practice-MOC]]"
  - "[[06-Patterns-MOC]]"
  - "[[Easy-Questions]]"
  - "[[Medium-Questions]]"
  - "[[Hard-Questions]]"
aliases:
  - Pattern Question Map
  - Pattern to questions
---


# Pattern → Question Map

Every [[06-Patterns-MOC|pattern]] with its representative questions, easiest → hardest. Drill one row at a time until the smell is automatic.

## [[Two-Pointers]]
- [[Two-Sum-II-Input-Array-Is-Sorted]] (easy) · [[Valid-Palindrome]] (easy) · [[Container-With-Most-Water]] (medium) · [[3Sum]] (medium) · [[Trapping-Rain-Water]] (hard)

## [[Sliding-Window]]
- [[Maximum-Average-Subarray-I]] (easy) · [[Longest-Substring-Without-Repeating-Characters]] (medium) · [[Fruit-Into-Baskets]] (medium) · [[Permutation-in-String]] (medium) · [[Minimum-Window-Substring]] (medium) · [[Sliding-Window-Maximum]] (hard)

## [[Fast-Slow-Pointers]]
- [[Middle-of-the-Linked-List]] (easy) · [[Linked-List-Cycle]] (easy) · [[Linked-List-Cycle-II]] (medium) · [[Happy-Number]] (easy) · [[Find-the-Duplicate-Number]] (medium)

## [[Merge-Intervals]]
- [[Meeting-Rooms]] (easy) · [[Merge-Intervals]] (medium) · [[Insert-Interval]] (medium) · [[Meeting-Rooms-II]] (medium) · [[Non-overlapping-Intervals]] (medium) · [[Employee-Free-Time]] (hard)

## [[Cyclic-Sort]]
- [[Missing-Number]] (easy) · [[Find-All-Numbers-Disappeared-in-an-Array]] (easy) · [[Find-the-Duplicate-Number]] (medium) · [[First-Missing-Positive]] (hard)

## [[In-Place-Reversal]]
- [[Reverse-Linked-List]] (easy) · [[Reverse-Linked-List-II]] (medium) · [[Reverse-Nodes-in-k-Group]] (hard) · [[Rotate-Array]] (medium) · [[Reverse-Words-in-a-String]] (medium)

## [[Tree-BFS]]
- [[Binary-Tree-Level-Order-Traversal]] (medium) · [[Binary-Tree-Right-Side-View]] (medium) · [[Binary-Tree-Zigzag-Level-Order-Traversal]] (medium) · [[Populating-Next-Right-Pointers]] (medium) · [[Minimum-Depth-of-Binary-Tree]] (easy)

## [[Tree-DFS]]
- [[Maximum-Depth-of-Binary-Tree]] (easy) · [[Same-Tree]] (easy) · [[Path-Sum]] (easy) · [[Validate-Binary-Search-Tree]] (medium) · [[Diameter-of-Binary-Tree]] (medium) · [[Binary-Tree-Maximum-Path-Sum]] (hard) · [[Serialize-and-Deserialize-Binary-Tree]] (hard)

## [[Subsets]]
- [[Subsets]] (medium) · [[Subsets-II]] (medium) · [[Permutations]] (medium) · [[Combinations]] (medium) · [[Combination-Sum]] (medium) · [[Generate-Parentheses]] (medium) · [[Letter-Case-Permutation]] (medium)

## [[Graph-BFS]]
- [[Number-of-Islands]] (medium) · [[Rotting-Oranges]] (medium) · [[01-Matrix]] (medium) · [[Walls-and-Gates]] (medium) · [[Word-Ladder]] (hard) · [[Shortest-Path-in-Binary-Matrix]] (medium)

## [[Topological-Sort]]
- [[Course-Schedule]] (medium) · [[Course-Schedule-II]] (medium) · [[Alien-Dictionary]] (hard) · [[Parallel-Courses]] (medium) · [[Sequence-Reconstruction]] (hard)

## [[K-Way-Merge]]
- [[Merge-K-Sorted-Lists]] (hard) · [[Kth-Smallest-Element-in-a-Sorted-Matrix]] (medium) · [[K-Closest-Points-to-Origin]] (medium) · [[Smallest-Range-Covering-Elements-from-K-Lists]] (hard) · [[Find-K-Pairs-with-Smallest-Sums]] (medium)

## [[Monotonic-Stack]]
- [[Next-Greater-Element-I]] (easy) · [[Daily-Temperatures]] (medium) · [[Stock-Span-Problem]] (medium) · [[Largest-Rectangle-in-Histogram]] (hard) · [[Maximal-Rectangle]] (hard) · [[Sum-of-Subarray-Minimums]] (hard) · [[Remove-K-Digits]] (medium)

## [[Prefix-Sum]]
- [[Running-Sum-of-1d-Array]] (easy) · [[Find-Pivot-Index]] (easy) · [[Range-Sum-Query-Immutable]] (easy) · [[Subarray-Sum-Equals-K]] (medium) · [[Continuous-Subarray-Sum]] (medium) · [[Range-Sum-Query-2D-Immutable]] (medium) · [[Car-Pooling]] (medium, difference array)

## Cross-cutting / algorithm notes
- [[Searching]] → [[Binary-Search]], [[Search-in-Rotated-Sorted-Array]], [[Koko-Eating-Bananas]], [[Split-Array-Largest-Sum]]
- [[Sorting]] → [[Sort-an-Array]], [[Kth-Largest-Element-in-an-Array]], [[Sort-Colors]]
- [[Recursion]] → [[Climbing-Stairs]], [[Subsets]], [[Maximum-Depth-of-Binary-Tree]]
- [[Backtracking]] → [[Permutations]], [[N-Queens]], [[Sudoku-Solver]], [[Word-Search]], [[Word-Search-II]]
- [[Divide-and-Conquer]] → [[Sort-an-Array]], [[Merge-K-Sorted-Lists]], [[Majority-Element]], [[Count-of-Smaller-Numbers-After-Self]]
- [[Greedy]] → [[Jump-Game]], [[Non-overlapping-Intervals]], [[Assign-Cookies]], [[Task-Scheduler]], [[Gas-Station]]
- [[DP]] → [[Climbing-Stairs]], [[House-Robber]], [[Coin-Change]], [[Longest-Increasing-Subsequence]], [[Longest-Common-Subsequence]], [[Edit-Distance]], [[Word-Break]], [[Burst-Balloons]], [[Palindrome-Partitioning-II]]
- [[Bit-Manipulation]] → [[Single-Number]], [[Number-of-1-Bits]], [[Counting-Bits]], [[Missing-Number]], [[Maximum-XOR-of-Two-Numbers-in-an-Array]]
- [[Math]] → [[Missing-Number]], [[Pow-x-n]], [[Count-Primes]], [[Happy-Number]], [[Roman-to-Integer]]

> [!tip] The "Multiple patterns" hard problems
> Many hard problems admit **several** patterns. [[Trapping-Rain-Water]] → [[Two-Pointers]] **or** [[Prefix-Sum]] **or** [[Monotonic-Stack]]. [[Find-the-Duplicate-Number]] → [[Cyclic-Sort]] **or** [[Fast-Slow-Pointers]] **or** [[Bit-Manipulation]] **or** [[Binary-Search]]-on-value. Learning multiple solutions per problem deepens pattern intuition.

## Questions grouped by pattern (static)
### None
| Question | Difficulty |
| --- | --- |
| [[Cherry-Pickup]] | hard |
| [[Critical-Connections-in-a-Network]] | hard |
| [[Distinct-Subsequences]] | hard |
| [[Evaluate-Reverse-Polish-Notation]] | medium |
| [[Kth-Smallest-Element-in-a-BST]] | medium |
| [[Longest-Repeating-Character-Replacement]] | medium |
| [[Lowest-Common-Ancestor-of-a-Binary-Tree]] | medium |
| [[Min-Stack]] | medium |
| [[Palindrome-Partitioning]] | hard |
| [[Partition-Equal-Subset-Sum]] | medium |
| [[Path-Sum-II]] | medium |
| [[Regular-Expression-Matching]] | hard |
| [[Unique-Paths]] | medium |
| [[Word-Ladder-II]] | hard |

### [[06-Patterns-Techniques/Merge-Intervals|Merge-Intervals]]
| Question | Difficulty |
| --- | --- |
| [[Employee-Free-Time]] | medium |
| [[Insert-Interval]] | medium |
| [[Meeting-Rooms-II]] | medium |
| [[Meeting-Rooms]] | medium |
| [[Merge-Intervals]] | medium |
| [[Non-overlapping-Intervals]] | medium |

### [[06-Patterns-Techniques/Subsets|Subsets]]
| Question | Difficulty |
| --- | --- |
| [[Combination-Sum]] | medium |
| [[Combinations]] | medium |
| [[Generate-Parentheses]] | medium |
| [[Letter-Case-Permutation]] | medium |
| [[Permutations]] | medium |
| [[Subsets-II]] | medium |
| [[Subsets]] | medium |

### [[Backtracking]]
| Question | Difficulty |
| --- | --- |
| [[N-Queens]] | hard |
| [[Sudoku-Solver]] | hard |
| [[Word-Search-II]] | hard |
| [[Word-Search]] | medium |

### [[Bit-Manipulation]]
| Question | Difficulty |
| --- | --- |
| [[Counting-Bits]] | medium |
| [[Maximum-XOR-of-Two-Numbers-in-an-Array]] | hard |
| [[Number-of-1-Bits]] | easy |
| [[Single-Number]] | easy |

### [[Cyclic-Sort]]
| Question | Difficulty |
| --- | --- |
| [[Find-All-Numbers-Disappeared-in-an-Array]] | medium |
| [[First-Missing-Positive]] | hard |
| [[Missing-Number]] | easy |

### [[DP]]
| Question | Difficulty |
| --- | --- |
| [[Burst-Balloons]] | hard |
| [[Climbing-Stairs]] | easy |
| [[Coin-Change]] | medium |
| [[Edit-Distance]] | hard |
| [[House-Robber]] | medium |
| [[Longest-Common-Subsequence]] | medium |
| [[Longest-Increasing-Subsequence]] | medium |
| [[Palindrome-Partitioning-II]] | hard |
| [[Word-Break]] | medium |

### [[Divide-and-Conquer]]
| Question | Difficulty |
| --- | --- |
| [[Count-of-Smaller-Numbers-After-Self]] | hard |
| [[Majority-Element]] | medium |
| [[Sort-an-Array]] | medium |

### [[Fast-Slow-Pointers]]
| Question | Difficulty |
| --- | --- |
| [[Find-the-Duplicate-Number]] | hard |
| [[Happy-Number]] | easy |
| [[Linked-List-Cycle-II]] | medium |
| [[Linked-List-Cycle]] | easy |
| [[Middle-of-the-Linked-List]] | easy |

### [[Graph-BFS]]
| Question | Difficulty |
| --- | --- |
| [[01-Matrix]] | medium |
| [[Clone-Graph]] | medium |
| [[Number-of-Islands]] | medium |
| [[Rotting-Oranges]] | medium |
| [[Shortest-Path-in-Binary-Matrix]] | medium |
| [[Walls-and-Gates]] | medium |
| [[Word-Ladder]] | medium |

### [[Greedy]]
| Question | Difficulty |
| --- | --- |
| [[Assign-Cookies]] | medium |
| [[Gas-Station]] | medium |
| [[Jump-Game]] | medium |
| [[Task-Scheduler]] | medium |

### [[HashMap]]
| Question | Difficulty |
| --- | --- |
| [[First-Unique-Character-in-a-String]] | easy |

### [[In-Place-Reversal]]
| Question | Difficulty |
| --- | --- |
| [[Reverse-Linked-List-II]] | medium |
| [[Reverse-Linked-List]] | easy |
| [[Reverse-Nodes-in-k-Group]] | hard |
| [[Reverse-Words-in-a-String]] | medium |
| [[Rotate-Array]] | medium |

### [[K-Way-Merge]]
| Question | Difficulty |
| --- | --- |
| [[Find-K-Pairs-with-Smallest-Sums]] | medium |
| [[Find-Median-from-Data-Stream]] | hard |
| [[K-Closest-Points-to-Origin]] | medium |
| [[Kth-Smallest-Element-in-a-Sorted-Matrix]] | medium |
| [[Merge-K-Sorted-Lists]] | hard |
| [[Smallest-Range-Covering-Elements-from-K-Lists]] | hard |

### [[Linked-List]]
| Question | Difficulty |
| --- | --- |
| [[Merge-Two-Sorted-Lists]] | easy |
| [[Palindrome-Linked-List]] | hard |
| [[Remove-Nth-Node-From-End]] | medium |

### [[Math]]
| Question | Difficulty |
| --- | --- |
| [[Count-Primes]] | medium |
| [[Pow-x-n]] | medium |
| [[Roman-to-Integer]] | easy |

### [[Monotonic-Stack]]
| Question | Difficulty |
| --- | --- |
| [[Daily-Temperatures]] | medium |
| [[Largest-Rectangle-in-Histogram]] | hard |
| [[Maximal-Rectangle]] | hard |
| [[Next-Greater-Element-I]] | medium |
| [[Remove-K-Digits]] | medium |
| [[Stock-Span-Problem]] | medium |
| [[Sum-of-Subarray-Minimums]] | hard |

### [[Prefix-Sum]]
| Question | Difficulty |
| --- | --- |
| [[Car-Pooling]] | medium |
| [[Continuous-Subarray-Sum]] | medium |
| [[Find-Pivot-Index]] | medium |
| [[Group-Anagrams]] | medium |
| [[Longest-Consecutive-Sequence]] | medium |
| [[Range-Sum-Query-2D-Immutable]] | medium |
| [[Range-Sum-Query-Immutable]] | medium |
| [[Running-Sum-of-1d-Array]] | medium |
| [[Subarray-Sum-Equals-K]] | medium |
| [[Top-K-Frequent-Elements]] | medium |

### [[Searching]]
| Question | Difficulty |
| --- | --- |
| [[Binary-Search]] | medium |
| [[Koko-Eating-Bananas]] | medium |
| [[Search-in-Rotated-Sorted-Array]] | medium |
| [[Split-Array-Largest-Sum]] | medium |

### [[Sliding-Window]]
| Question | Difficulty |
| --- | --- |
| [[Fruit-Into-Baskets]] | medium |
| [[Longest-Substring-Without-Repeating-Characters]] | medium |
| [[Maximum-Average-Subarray-I]] | medium |
| [[Minimum-Window-Substring]] | medium |
| [[Permutation-in-String]] | medium |
| [[Sliding-Window-Maximum]] | hard |

### [[Sorting]]
| Question | Difficulty |
| --- | --- |
| [[Kth-Largest-Element-in-an-Array]] | medium |
| [[Sort-Colors]] | medium |

### [[Stack]]
| Question | Difficulty |
| --- | --- |
| [[Implement-StrStr]] | easy |
| [[Longest-Common-Prefix]] | easy |
| [[Valid-Parentheses]] | easy |

### [[Topological-Sort]]
| Question | Difficulty |
| --- | --- |
| [[Alien-Dictionary]] | medium |
| [[Course-Schedule-II]] | medium |
| [[Course-Schedule]] | medium |
| [[Parallel-Courses]] | medium |
| [[Sequence-Reconstruction]] | medium |

### [[Tree-BFS]]
| Question | Difficulty |
| --- | --- |
| [[Binary-Tree-Level-Order-Traversal]] | medium |
| [[Binary-Tree-Right-Side-View]] | medium |
| [[Binary-Tree-Zigzag-Level-Order-Traversal]] | medium |
| [[Minimum-Depth-of-Binary-Tree]] | medium |
| [[Populating-Next-Right-Pointers]] | medium |

### [[Tree-DFS]]
| Question | Difficulty |
| --- | --- |
| [[Binary-Tree-Maximum-Path-Sum]] | hard |
| [[Diameter-of-Binary-Tree]] | medium |
| [[Invert-Binary-Tree]] | easy |
| [[Maximum-Depth-of-Binary-Tree]] | easy |
| [[Path-Sum]] | medium |
| [[Same-Tree]] | easy |
| [[Serialize-and-Deserialize-Binary-Tree]] | hard |
| [[Symmetric-Tree]] | easy |
| [[Validate-Binary-Search-Tree]] | medium |

### [[Two-Pointers]]
| Question | Difficulty |
| --- | --- |
| [[3Sum]] | medium |
| [[Best-Time-to-Buy-and-Sell-Stock]] | easy |
| [[Container-With-Most-Water]] | medium |
| [[Contains-Duplicate]] | easy |
| [[Move-Zeroes]] | easy |
| [[Remove-Duplicates-from-Sorted-Array]] | easy |
| [[Trapping-Rain-Water]] | hard |
| [[Two-Sum-II-Input-Array-Is-Sorted]] | easy |
| [[Two-Sum]] | easy |
| [[Valid-Palindrome]] | easy |
| [[Question-Template]] | medium |

