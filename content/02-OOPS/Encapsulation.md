---
type: concept
tags:
  - java/oops
  - java/encapsulation
difficulty: easy
status: evergreen
related:
  - "[[Classes-and-Objects]]"
  - "[[Object-Class]]"
  - "[[SOLID-Principles]]"
aliases:
  - Encapsulation
  - Access modifiers
---


# Encapsulation

> [!note] Definition
> Encapsulation bundles data (fields) with the methods that act on it, and **restricts direct access** to the data via access modifiers. The first pillar of OOP: protect invariants by funnelling changes through validated methods.

## Access modifiers
| Modifier | same class | same package | subclass | world |
|----------|:---:|:---:|:---:|:---:|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| _(default/package)_ | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

## Idiom: private fields + accessors
```java
class Account {
    private double balance;          // hidden

    public double getBalance() { return balance; }
    public void deposit(double amt) {
        if (amt <= 0) throw new IllegalArgumentException("amt>0");
        balance += amt;              // invariant maintained
    }
    public void withdraw(double amt) {
        if (amt > balance) throw new IllegalStateException("overdraft");
        balance -= amt;
    }
}
```

## Why it matters
- **Validation gate**: setters prevent illegal states (`balance >= 0`).
- **Representation freedom**: switch `balance` from `double` to `int` cents without breaking callers.
- **Concurrency control**: a synchronized accessor can guard shared state ([[Multithreading]]).

> [!tip] Default posture
> Make fields `private` by default. Expose behavior through methods. Widen access only when you have a reason. This is the foundation of [[SOLID-Principles]] and prevents "anemic" data-bag classes.

## Encapsulation as a protective shell
```
Client code
    │
    ├─calls──>  deposit(amt)  ──validates──>  private balance
    │                                              ▲
    └─calls──>  withdraw(amt) ──validates───────┘

Direct access to private balance: BLOCKED
```

## POJO vs rich object
| Style | Characteristics | Verdict |
|-------|-----------------|---------|
| Anemic POJO | public getters/setters for every field | Leaks invariants |
| Rich object | private fields + behavior methods | Maintains invariants |
| Record | compact data carrier | Great for immutable DTOs |

## Encapsulation in collections
```java
private List<Item> items = new ArrayList<>();

public List<Item> getItems() {
    return Collections.unmodifiableList(items); // safe view
}
```
Returning the raw list lets callers clear it — breaking encapsulation.

> [!warning] Pitfalls
> - Encapsulation ≠ just getters/setters for every field (that leaks structure). Prefer **behavior methods** (`deposit`, not `setBalance`).
> - Returning an internal mutable collection field exposes state: return a copy or an unmodifiable view (`Collections.unmodifiableList`).
> - `protected` is accessible to the whole **package** too, not just subclasses — often surprising.

## Related
- [[Classes-and-Objects]] · [[Object-Class]] · [[SOLID-Principles]]
