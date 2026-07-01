---
type: concept
tags:
  - java/advanced
  - java/streams
  - java/lambda
difficulty: medium
pattern: ""
related:
  - "[[Collections-Framework]]"
  - "[[Generics]]"
  - "[[Inner-Classes]]"
  - "[[Sorting]]"
aliases:
  - Streams
  - Lambdas
  - Functional interfaces
---

# Streams & Lambdas

> [!note] Definition
> **Lambdas** are concise anonymous functions for functional interfaces (one abstract method). **Streams** are declarative, lazy pipelines over data sources (collections, arrays, I/O) supporting map/filter/reduce style. Together they enable a functional flavor in Java.

## Functional interfaces & lambdas
```java
@FunctionalInterface interface Mapper<T,R> { R map(T t); }
Mapper<String,Integer> len = s -> s.length();      // lambda
Supplier<String> greeting = () -> "hi";
Comparator<String> byLen = (a,b) -> a.length() - b.length();
```
Built-ins in `java.util.function`: `Function<T,R>`, `Predicate<T>`, `Consumer<T>`, `Supplier<T>`, `BiFunction<T,U,R>`, plus primitive specializations (`IntFunction`, `ToIntFunction`...).

## Method references (shorthand)
```java
list.stream().map(String::length)        // instance method ref
list.stream().forEach(System.out::println)
list.stream().mapToInt(Integer::intValue)
Stream.generate(() -> "x")               // supplier
```

## Stream pipeline
```java
List<String> names = List.of("ann","bob","carl","ava");

long count = names.stream()                       // source
    .filter(s -> s.length() == 3)                 // intermediate (lazy)
    .map(String::toUpperCase)                     // intermediate
    .distinct()
    .sorted()
    .count();                                     // terminal (triggers work)

// reduce
int sum = nums.stream().reduce(0, Integer::sum);
Optional<String> longest = names.stream().max(Comparator.comparingInt(String::length));

// collect to structures
List<Integer> evens = nums.stream().filter(n -> n%2==0).toList();   // Java 16+ unmodifiable
Map<Character,List<String>> byFirst =
    names.stream().collect(Collectors.groupingBy(s -> s.charAt(0)));
String joined = names.stream().collect(Collectors.joining(", "));
```

## Numeric streams
```java
IntStream.range(0, n).sum();
IntStream.of(arr).max().orElseThrow();
nums.stream().mapToInt(Integer::intValue).sum();   // avoid boxing
```

## `Optional` — a typed "maybe"
```java
Optional<String> opt = Optional.ofNullable(getName());
String s = opt.map(String::toUpperCase).orElse("DEFAULT");
opt.ifPresent(System.out::println);
opt.orElseThrow();   // unwrap or throw
```
Use it as a return type to force callers to handle "no value."

> [!tip] DSA usage (sparingly!)
> Streams shine in **input parsing and result shaping**, less in hot loops:
> ```java
> int[] a = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();
> Arrays.sort(arr);                                       // imperative sort is faster
> Map<Integer,Integer> freq = Arrays.stream(arr).boxed()
>     .collect(Collectors.toMap(x->x, x->1, Integer::sum));
> ```
> Prefer explicit loops for performance-critical code — streams add overhead and obscure complexity.

> [!warning] Pitfalls
> - Streams are **single-use**; can't replay a stream after a terminal op.
> - Side effects in lambdas (mutating outer state) defeat the model and can break parallel streams.
> - `forEach` is for side effects, not for "mapping in place"; use `map` + `toList`.
> - `parallelStream()` rarely helps small data and breaks order/nonatomic assumptions — don't reach for it by default.

## Related
- [[Collections-Framework]] · [[Generics]] · [[Inner-Classes]] (anonymous → lambda) · [[Sorting]] (comparators)
