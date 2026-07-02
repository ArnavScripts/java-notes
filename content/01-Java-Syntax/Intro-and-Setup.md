---
type: concept
tags:
  - java/syntax
  - java/setup
difficulty: easy
status: evergreen
related:
  - "[[Variables-and-Data-Types]]"
  - "[[Methods]]"
  - "[[JVM-and-Memory]]"
aliases:
  - Hello World
  - Java Setup
---


# Intro & Setup

> [!note] Definition
> Java is a compiled-then-interpreted, statically-typed, object-oriented language. Source `.java` → `javac` → bytecode `.class` → `java` → JVM executes.

## Why it matters
- "Write once, run anywhere" — bytecode runs on any JVM.
- Understand the toolchain to debug errors like `ClassNotFoundException`.

## The three layers
- **JDK** (Java Development Kit) = JRE + dev tools (`javac`). What you install to develop.
- **JRE** (Java Runtime Environment) = JVM + core libraries. What you need to run.
- **JVM** (Java Virtual Machine) = executes bytecode. See [[JVM-and-Memory]].

## Hello world
```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
Compile & run:
```bash
javac Hello.java   # produces Hello.class
java Hello         # runs main()
```

## Anatomy of `main`
`public static void main(String[] args)`
- `public` — JVM can call from outside the class
- `static` — no object needed to start
- `void` — returns nothing
- `String[] args` — command-line args

> [!warning] Pitfalls
> - File name **must** match the `public class` name (case-sensitive).
> - `System.out.println` adds a newline; `System.out.print` does not.

## The Java toolchain flow
```
Hello.java  --javac-->  Hello.class  --java-->  JVM  -->  Output
```

## JVM, JRE, JDK relationship
```
JDK
├── JRE
│   ├── JVM
│   └── Core libraries
└── Dev tools: javac, javadoc, jar
```

## Common commands
| Command | Purpose | Output |
|---------|---------|--------|
| `javac Hello.java` | Compile source | `Hello.class` |
| `java Hello` | Run bytecode | Program execution |
| `java -version` | Check installed version | Version string |
| `javap -c Hello` | Disassemble bytecode | Instruction listing |

## One-file rule
A `.java` file may contain multiple classes, but at most **one** public class, and its filename must match that public class.

## Related
- [[Variables-and-Data-Types]] — next: types
- [[Methods]] — why `static` matters
