---
type: concept
tags:
  - java/syntax
  - java/io
difficulty: easy
pattern: ""
related:
  - "[[Strings]]"
  - "[[Exception-Basics]]"
  - "[[IO-and-NIO]]"
aliases:
  - Scanner
  - BufferedReader
---

# Input / Output

> [!note] Definition
> Read tokens/lines from stdin and write to stdout. For DSA contests use **BufferedReader + PrintWriter** for speed; for quick scripts use **Scanner**.

## Scanner (simple, slow)
```java
import java.util.Scanner;
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();
String word = sc.next();        // next whitespace-delimited token
String line = sc.nextLine();    // rest of current line
double d = sc.nextDouble();
```
> [!warning] After `nextInt()` then `nextLine()`, the `nextLine()` reads the leftover newline → empty. Read the full line and parse, or add an extra `nextLine()`.

## BufferedReader (fast)
```java
import java.io.*;
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String line = br.readLine();              // null at EOF
int n = Integer.parseInt(br.readLine().trim());
String[] parts = br.readLine().split(" ");
int[] a = new int[parts.length];
for (int i = 0; i < parts.length; i++) a[i] = Integer.parseInt(parts[i]);
```

## PrintWriter (fast output)
```java
PrintWriter out = new PrintWriter(new BufferedOutputStream(System.out));
out.println(result);
out.flush();    // MUST flush/close or output is lost
```
`StringBuilder` batching + one print is even faster for many small outputs.

## Parsing helpers
```java
Integer.parseInt(s)   Long.parseLong(s)   Double.parseDouble(s)
```

> [!tip] Contest setup boilerplate
> ```java
> public class Main {
>     public static void main(String[] args) throws IOException {
>         BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
>         PrintWriter out = new PrintWriter(new BufferedOutputStream(System.out));
>         // ... solve ...
>         out.flush();
>     }
> }
> ```

## Related
- [[Strings]] · [[Exception-Basics]] (why `throws IOException`) · [[IO-and-NIO]] (file I/O)
