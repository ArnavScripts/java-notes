---
type: moc
tags:
  - moc
  - dsa/pattern
status: evergreen
related:
  - "[[00-Index-MOC]]"
  - "[[05-Algorithms-MOC]]"
  - "[[04-Data-Structures-MOC]]"
  - "[[07-Practice-MOC]]"
aliases:
  - Patterns MOC
  - Techniques MOC
---


# Patterns & Techniques — MOC

The **pattern-recognition core** of DSA. Most interview/contest problems are instances of a handful of reusable templates. Learn the smell of each, and a "new" problem becomes a known one.

> [!tip] How to study patterns
> For each: read the **cues** (when it applies), study the **template**, solve the linked questions, then ask on every new problem *"which pattern does this smell like?"* Many problems combine 2+ patterns.

## Patterns
- [[Two-Pointers]] — two indices converge / scan
- [[Sliding-Window]] — contiguous subarray/substring with constraint
- [[Fast-Slow-Pointers]] — Floyd, cycle/middle on lists
- [[Merge-Intervals]] — overlapping ranges
- [[Cyclic-Sort]] — place 1..n at their indices
- [[In-Place-Reversal]] — reverse list/array portions
- [[Tree-BFS]] — level-order traversal
- [[Tree-DFS]] — preorder/inorder/postorder + path logic
- [[Subsets]] — power set / combinations via choose-unchoose
- [[Graph-BFS]] — shortest path on unweighted graphs/grids
- [[Topological-Sort]] — order with dependencies (DAG)
- [[K-Way-Merge]] — merge k sorted streams via heap
- [[Monotonic-Stack]] — next greater/smaller, spans
- [[Prefix-Sum]] — O(1) range sums / count diffs

## Smell → pattern quick map
| Problem cue | Pattern |
|-------------|---------|
| "sorted array, pair/triplet sum" | [[Two-Pointers]] |
| "longest/shortest contiguous subarray with constraint" | [[Sliding-Window]] |
| "cycle in list / find middle / happy number" | [[Fast-Slow-Pointers]] |
| "overlapping meetings / insert interval" | [[Merge-Intervals]] |
| "array of 1..n, missing/duplicate" | [[Cyclic-Sort]] |
| "reverse every k nodes / reverse sublist" | [[In-Place-Reversal]] |
| "level by level / right side view" | [[Tree-BFS]] |
| "max depth / path sum / validate" | [[Tree-DFS]] |
| "all subsets / combinations" | [[Subsets]] |
| "shortest path / fewest moves / islands" | [[Graph-BFS]] |
| "prerequisites / build order / course schedule" | [[Topological-Sort]] |
| "merge k sorted lists/arrays" | [[K-Way-Merge]] |
| "next greater element / stock span / largest rectangle" | [[Monotonic-Stack]] |
| "sum of subarray [l..r] / count subarrays with sum k" | [[Prefix-Sum]] |

## All pattern notes
| Pattern | Difficulty | Status |
| --- | --- | --- |
| [[Cyclic-Sort]] | medium | evergreen |
| [[Fast-Slow-Pointers]] | medium | evergreen |
| [[Graph-BFS]] | hard | evergreen |
| [[In-Place-Reversal]] | medium | evergreen |
| [[K-Way-Merge]] | hard | evergreen |
| [[Merge-Intervals]] | medium | evergreen |
| [[Monotonic-Stack]] | hard | evergreen |
| [[Prefix-Sum]] | medium | evergreen |
| [[Sliding-Window]] | medium | evergreen |
| [[Subsets]] | medium | evergreen |
| [[Topological-Sort]] | hard | evergreen |
| [[Tree-BFS]] | medium | evergreen |
| [[Tree-DFS]] | hard | evergreen |
| [[Two-Pointers]] | easy | evergreen |

## Thin pattern notes
*No entries.*

## Questions grouped by pattern
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

