---
type: question
tags:
  - practice
  - medium
  - dp
difficulty: medium
pattern: "[[DP]]"
source: "LeetCode 322"
related:
  - "[[DP]]"
  - "[[Recursion]]"
  - "[[Greedy]]"
aliases:
  - Coin Change
---

# Coin Change

> [!note] Problem
> Given coins of denominations `coins` and an amount `amount`, return the **fewest number of coins** to make that amount, or `-1` if impossible. Unlimited supply of each coin.

## Examples
```
Input:  coins = [1,2,5], amount = 11
Output: 3              // 5 + 5 + 1

Input:  coins = [2], amount = 3
Output: -1
```

## Brute force
- Try every combination via recursion → exponential.

## Optimal approach
- Pattern: [[DP]] (unbounded knapsack, 1D tabulation)
- Idea: `dp[a]` = min coins to make amount `a`. For each amount, try every coin: `dp[a] = min(dp[a], dp[a - coin] + 1)`.
- Steps:
  1. `dp[0..amount] = INF`, `dp[0] = 0`.
  2. For `a` in 1..amount: for each coin ≤ a: `dp[a] = min(dp[a], dp[a-coin] + 1)`.
  3. Return `dp[amount]` unless INF → `-1`.

> [!tip] This is the **unbounded** knapsack shape — each coin can be reused, so the inner loop over coins runs forward (no "use once" reverse trick). Contrast with 0/1 knapsack.

## Complexity
- Time: O(amount · coins) · Space: O(amount)

## Java solution
```java
public int coinChange(int[] coins, int amount) {
    int INF = amount + 1;
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, INF);
    dp[0] = 0;
    for (int a = 1; a <= amount; a++)
        for (int c : coins)
            if (c <= a) dp[a] = Math.min(dp[a], dp[a - c] + 1);
    return dp[amount] == INF ? -1 : dp[amount];
}
```

> [!warning] Greedy (always take the largest coin) **fails** for arbitrary denominations — e.g. coins `{1,3,4}`, amount 6: greedy gives 4+1+1 = 3 coins, optimal is 3+3 = 2. Only canonical systems allow greedy. Default to DP.

## Related
- [[DP]] · [[Recursion]] (memoized variant) · [[Partition-Equal-Subset-Sum]] (0/1 knapsack)
