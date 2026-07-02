---
type: concept
tags:
  - dsa/ds
  - java/ds/trie
difficulty: hard
status: evergreen
related:
  - "[[Strings-DS]]"
  - "[[HashMap]]"
  - "[[Tree-DFS]]"
  - "[[Backtracking]]"
aliases:
  - Trie
  - Prefix Tree
---


# Trie

> [!note] Definition
> A trie (prefix tree) stores strings character-by-character along paths from the root. Shared prefixes share nodes → great for autocomplete, dictionary lookup, prefix queries. Search/insert is O(L) where L = word length, independent of dictionary size.

## Node model
```java
static class Trie {
    Trie[] kids = new Trie[26];   // R-ary; use HashMap for large alphabets
    boolean end;
    void insert(String w){
        Trie c = this;
        for (char ch : w.toCharArray()){
            int i = ch - 'a';
            if (c.kids[i] == null) c.kids[i] = new Trie();
            c = c.kids[i];
        }
        c.end = true;
    }
    boolean search(String w){ Trie n = walk(w); return n != null && n.end; }
    boolean startsWith(String p){ return walk(p) != null; }
    Trie walk(String s){
        Trie c = this;
        for (char ch : s.toCharArray()){
            int i = ch - 'a';
            if (c.kids[i] == null) return null;
            c = c.kids[i];
        }
        return c;
    }
}
```

## Complexity
- Insert / search / delete: O(L) time, O(Σ·N) space (Σ = alphabet, N = total chars stored).
- For lowercase English: 26 children per node. For Unicode: `Map<Character,Trie>`.

## Variants
- **Compressed trie (radix/patricia)**: merge single-child chains → fewer nodes.
- **Ternary search tree**: `left/mid/right` like a BST per char — less memory, O(L + log Σ).
- **Suffix trie/tree**: all suffixes of a string — substring queries (heavy memory).
- **Bitwise trie**: over binary representation of ints — XOR-maximization, IP routing.

## Worked: word search with `.` wildcards
```java
boolean searchWild(Trie c, String w, int i){
    if (i == w.length()) return c.end;
    char ch = w.charAt(i);
    if (ch == '.') {
        for (Trie k : c.kids) if (k != null && searchWild(k, w, i+1)) return true;
        return false;
    }
    return c.kids[ch-'a'] != null && searchWild(c.kids[ch-'a'], w, i+1);
}
```

> [!tip] Pattern recognition cues
> - "Many words, prefix queries (autocomplete, starts-with)" → trie.
> - "Word dictionary with wildcard `.`" → trie + DFS.
> - "Word break / word search II on a board" → trie of the dictionary + grid DFS ([[Backtracking]]).
> - "Maximum XOR pair" → bitwise trie over bits MSB→LSB.
> - "Longest common prefix of many strings" → trie depth, or sort + compare neighbors.
> - If alphabet is tiny and words few, a `HashSet` + `startsWith` may suffice — don't over-engineer.

> [!warning] Pitfalls
  - Forgetting to set `end = true` (a prefix is not a stored word unless marked).
  - Memory blow-up: 26 children per node for millions of words — consider compression or a map.
  - Deletion must clear `end` and prune childless nodes to free memory.
  - Case-sensitivity: normalize case before inserting.

## Practice questions
- [[Implement-Trie]] → basic trie
- [[Word-Search-II]] → trie + grid DFS ([[Backtracking]])
- [[Design-Add-and-Search-Words]] → trie with wildcards
- [[Maximum-XOR-of-Two-Numbers]] → bitwise trie
- [[Longest-Word-in-Dictionary]] → trie + BFS/DFS

## Related
- [[Strings-DS]] · [[HashMap]] · [[Tree-DFS]] · [[Backtracking]]
