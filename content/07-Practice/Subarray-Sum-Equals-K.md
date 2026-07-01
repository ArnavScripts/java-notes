---
type: question
tags:
  - practice
  - medium
  - array
  - prefixsum
  - hashmap
difficulty: medium
pattern: "[[Prefix-Sum]]"
source: "LeetCode 560"
related:
  - "[[Prefix-Sum]]"
  - "[[HashMap]]"
  - "[[Arrays]]"
aliases:
  - Subarray Sum Equals K
---

# Subarray Sum Equals K

> [!note] Problem
> Given an integer array `nums` (may contain **negatives**) and an integer `k`, return the **number** of contiguous subarrays whose sum equals `k`.

## Examples
```
Input:  nums = [1,1,1], k = 2
Output: 2          // [1,1] at (0,1) and [1,1] at (1,2)

Input:  nums = [1,2,3], k = 3
Output: 2          // [1,2] and [3]
```

## Brute force
- Two nested loops computing subarray sums → O(n²).

## Optimal approach
- Pattern: [[Prefix-Sum]] + [[HashMap]]
- Idea: a subarray ending at i sums to k iff `prefix[i] - prefix[j] == k` for some earlier j. Count how many earlier prefixes equal `prefix[i] - k`.
- Steps:
  1. `Map<Integer,Integer> count` of prefix sums seen; seed with `count.put(0, 1)` (empty prefix).
  2. Walk, accumulate `sum`; add `count.getOrDefault(sum - k, 0)` to the answer; increment `count[sum]`.

> [!warning] [[Sliding-Window]] does **not** work here because negatives break monotonicity. Prefix-sum + hashmap is the canonical solution for "subarray sum = k with negatives".

## Complexity
- Time: O(n) · Space: O(n)

## Java solution
```java
public int subarraySum(int[] nums, int k) {
    Map<Integer,Integer> count = new HashMap<>();
    count.put(0, 1);
    int sum = 0, ans = 0;
    for (int x : nums) {
        sum += x;
        ans += count.getOrDefault(sum - k, 0);
        count.merge(sum, 1, Integer::sum);
    }
    return ans;
}
```

## Related
- [[Prefix-Sum]] · [[HashMap]] · [[Continuous-Subarray-Sum]] (mod variant)
