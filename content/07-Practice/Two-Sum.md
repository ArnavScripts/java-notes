---
type: question
tags:
  - dsa/ds/arrays
  - dsa/ds/hashmap
  - dsa/pattern/hashmap
  - practice
  - practice/easy
difficulty: easy
pattern: "[[HashMap]]"
status: evergreen
source: "LeetCode 1"
related:
  - "[[HashMap]]"
  - "[[Two-Pointers]]"
  - "[[Arrays]]"
aliases:
  - Two Sum
---


# Two Sum

> [!note] Problem
> Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`. Each input has exactly one solution and you may not use the same element twice.

## Examples
```
Input:  nums = [2,7,11,15], target = 9
Output: [0,1]              // 2 + 7 = 9

Input:  nums = [3,2,4], target = 6
Output: [1,2]              // 2 + 4 = 6
```

## Brute force
- Idea: try every pair (i, j) with i < j.
- Time: O(n²) · Space: O(1)

## Optimal approach
- Pattern: [[HashMap]] (one pass)
- Idea: as you scan, for each `nums[i]` check if `target - nums[i]` was already seen; the map stores value → index. One pass finds the pair because the complement of the later element is the earlier one.
- Steps:
  1. Create an empty `HashMap<Integer,Integer>` value→index.
  2. For each i: if `seen.containsKey(target - nums[i])` return `[seen.get(...), i]`; else `seen.put(nums[i], i)`.

## Complexity
- Time: O(n) — single pass, O(1) average map ops.
- Space: O(n) — for the map.

## Java solution
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer,Integer> seen = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        Integer j = seen.get(target - nums[i]);
        if (j != null) return new int[]{j, i};
        seen.put(nums[i], i);
    }
    return new int[]{-1, -1};   // per constraints, never reached
}
```

## Variants & links
- [[Two-Sum-II-Input-Array-Is-Sorted]] — input sorted → [[Two-Pointers]], O(1) space.
- [[3Sum]] — extend with sort + two pointers.
- [[Subarray-Sum-Equals-K]] — adjacent (subarray) instead of any two.

## Related
- [[HashMap]] · [[Two-Pointers]] · [[Arrays]]
