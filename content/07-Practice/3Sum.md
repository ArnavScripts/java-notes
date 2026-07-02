---
type: question
tags:
  - dsa/ds/arrays
  - dsa/pattern/two-pointers
  - practice
  - practice/medium
difficulty: medium
pattern: "[[Two-Pointers]]"
status: evergreen
source: "LeetCode 15"
related:
  - "[[Two-Pointers]]"
  - "[[Two-Sum-II-Input-Array-Is-Sorted]]"
  - "[[Sorting]]"
aliases:
  - 3Sum
  - Three Sum
---


# 3Sum

> [!note] Problem
> Given array `nums`, return all **unique** triplets `[nums[i], nums[j], nums[k]]` with `i != j != k` and `nums[i]+nums[j]+nums[k] == 0`. No duplicate triplets in the output.

## Examples
```
Input:  nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]     // (-1)+(-1)+2 = 0; (-1)+0+1 = 0
```

## Brute force
- Three nested loops + a set to dedupe → O(n³) time, O(1) extra (set for output).

## Optimal approach
- Pattern: [[Sorting]] + [[Two-Pointers]]
- Idea: sort, then for each fixed first element `nums[i]`, run the sorted [[Two-Sum-II-Input-Array-Is-Sorted]] two-pointer on the rest with target `-nums[i]`.
- Steps:
  1. Sort ascending.
  2. For `i` from 0: skip duplicates (`if i>0 && nums[i]==nums[i-1] continue`).
  3. `l=i+1, r=n-1`; move pointers by sum vs `-nums[i]`; on a hit, record and skip equal neighbors on both sides.

## Complexity
- Time: O(n²) — n outer × n inner two-pointer.
- Space: O(1) extra (ignoring output).

## Java solution
```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    int n = nums.length;
    for (int i = 0; i < n - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;          // dup first
        int l = i + 1, r = n - 1;
        while (l < r) {
            int s = nums[i] + nums[l] + nums[r];
            if (s == 0) {
                res.add(Arrays.asList(nums[i], nums[l], nums[r]));
                while (l < r && nums[l] == nums[l + 1]) l++;     // dup second
                while (l < r && nums[r] == nums[r - 1]) r--;     // dup third
                l++; r--;
            } else if (s < 0) l++; else r--;
        }
    }
    return res;
}
```

> [!tip] The duplicate-skips are the whole difficulty. After recording a triplet, advance past equal values on both sides so the same triplet isn't recorded again.

## Related
- [[Two-Pointers]] · [[Two-Sum-II-Input-Array-Is-Sorted]] · [[Sorting]] · [[4Sum]]
