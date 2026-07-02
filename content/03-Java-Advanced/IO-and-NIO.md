---
type: concept
tags:
  - java/advanced
  - java/io
difficulty: medium
status: evergreen
related:
  - "[[Input-Output]]"
  - "[[Exception-Handling]]"
  - "[[Strings]]"
aliases:
  - File I/O
  - NIO
---


# I/O & NIO

> [!note] Definition
> Two layers: classic `java.io` (byte/char **streams**, blocking) and `java.nio` (**channels + buffers**, can be non-blocking, `Path`-based file API). For files prefer `java.nio.file`; for stdin/stdout see [[Input-Output]].

## `Path` (NIO) — modern file paths
```java
import java.nio.file.*;
Path p = Paths.get("/tmp", "data", "a.txt");
p.getFileName(); p.getParent(); p.toAbsolutePath();
Path q = p.resolveSibling("b.txt");
Files.exists(p); Files.size(p);
```

## Reading & writing text (NIO helpers)
```java
// read all lines lazily (try-with-resources closes the stream)
try (var lines = Files.lines(p)) {
    lines.filter(s -> s.startsWith("#")).forEach(System.out::println);
}

// bulk read
List<String> all = Files.readAllLines(p);
String text = Files.readString(p);

// write
Files.write(p, List.of("a","b"));
Files.writeString(p, "hello\n");
```

## Classic byte/char streams
```java
// bytes
try (var in = new FileInputStream("f.bin")) { in.read(); }

// chars (bridge byte->char with encoding)
try (var r = new BufferedReader(new InputStreamReader(
        new FileInputStream("f.txt"), StandardCharsets.UTF_8))) {
    String line; while ((line = r.readLine()) != null) { /*...*/ }
}
try (var w = new PrintWriter(new OutputStreamWriter(
        new FileOutputStream("o.txt"), StandardCharsets.UTF_8))) {
    w.println("hi");
}
```

## Buffered I/O — why it matters
Unbuffered `read()/write()` hits the OS per byte → catastrophically slow. Wrapping in `BufferedInputStream/BufferedReader`/`BufferedOutputStream`/`PrintWriter` batches I/O → often 100×+ faster. Always buffer.

## Object serialization (legacy)
```java
try (var oos = new ObjectOutputStream(new FileOutputStream("o.ser"))) {
    oos.writeObject(obj);   // class must implement Serializable
}
```
> [!warning] Java serialization is insecure and brittle; prefer JSON (Jackson/Gson) or protobuf for new code. Use it only for quick local caching.

> [!tip] Pattern recognition
> - "Read a file line by line" → `Files.lines(p)` (lazy, memory-friendly).
> - "Read whole small file" → `Files.readString(p)`.
> - "Big binary stream" → buffered `InputStream` chunks.
> - Always wrap streams in try-with-resources — see [[Exception-Handling]].

> [!warning] Pitfalls
> - Forgetting to specify charset → platform-default surprise. Use `StandardCharsets.UTF_8`.
> - Not closing → resource leaks / locked files (Windows). try-with-resources fixes it.
> - `readLine()` strips the newline; `readAllLines` is eager (loads whole file).

## Related
- [[Input-Output]] (stdin/stdout) · [[Exception-Handling]] (try-with-resources) · [[Strings]]
