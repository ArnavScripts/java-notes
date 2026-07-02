---
type: concept
tags:
  - java/oops
  - java/enum
difficulty: medium
status: evergreen
related:
  - "[[Classes-and-Objects]]"
  - "[[static-Keyword]]"
  - "[[Abstraction-and-Interfaces]]"
aliases:
  - Enum
  - Enums
---


# Enums

> [!note] Definition
> An `enum` is a special class that defines a fixed set of type-safe singleton instances. Each constant is an object of the enum type. Far safer than `int`/`String` constants: the compiler checks values, and you can attach state and behavior.

## Basic enum
```java
enum Day { MON, TUE, WED, THU, FRI, SAT, SUN }
Day d = Day.MON;
Day[] all = Day.values();     // ordered array of constants
Day x = Day.valueOf("FRI");   // parse (throws on bad name)
int ord = d.ordinal();        // 0-based position (avoid relying on it)
switch (d) {
    case SAT: case SUN -> System.out.println("weekend");
    default            -> System.out.println("weekday");
}
```

## Enum with state & behavior
```java
enum Planet {
    MERCURY(3.303e23), VENUS(4.869e24), EARTH(5.976e24);

    private final double mass;            // each constant has its own
    Planet(double mass) { this.mass = mass; }   // constructor: package/private only
    public double mass() { return mass; }
}
Planet.EARTH.mass();    // 5.976e24
```

## Enum with abstract method (per-constant behavior)
```java
enum Op {
    PLUS  { public int apply(int a, int b){ return a+b; } },
    MINUS { public int apply(int a, int b){ return a-b; } };
    public abstract int apply(int a, int b);
}
Op.PLUS.apply(3,4);   // 7
```

## EnumSet / EnumMap (high performance)
```java
EnumSet<Day> workdays = EnumSet.range(Day.MON, Day.FRI);
EnumMap<Day,String> mood = new EnumMap<>(Day.class);
```
Internally bit-vector / array indexed by `ordinal()` → O(1), very compact.

> [!tip] DSA / design usage
> - Encoding directions (UP/DOWN/LEFT/RIGHT) or states (PLAYING/PAUSED/GAMEOVER) → use an enum, not magic ints.
> - Grid DFS with `enum Dir { U(-1,0), D(1,0), L(0,-1), R(0,1); ... }` keeps neighbor loops clean. See [[Tree-DFS]]/[[Graph-BFS]].

> [!warning] Pitfalls
> - `ordinal()` changes if you reorder constants → never persist it. Store the name (`name()`) instead.
> - Enum constructors can't be `public`; the JVM creates the constants at class-load time.
> - `valueOf` is case-sensitive and throws `IllegalArgumentException` on miss — validate input first.

## Related
- [[Classes-and-Objects]] · [[static-Keyword]] · [[Abstraction-and-Interfaces]] · [[Tree-DFS]]
