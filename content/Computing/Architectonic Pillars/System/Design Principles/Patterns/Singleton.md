# Singleton

**Summary**: Covers Singleton as a design principle or engineering pattern, with practical implications and examples.
**Tags**: #system-design #design-principles #singleton
**Created**: Unknown
**Last Updated**: 2026-05-20

---

← [[Design Pattern]] · [[00. Overview]]

# Singleton Pattern
**Category:** Creational · **Layer:** L2 Patterns

---

## 1. Problem & motivation

**Cues / Questions (left)**

- What problem does Singleton solve?
- Why is a global variable not enough?
- Where do we need exactly one instance?

**Notes (right)**

- Design patterns solve recurring design problems; Singleton solves: "How to ensure only **one** instance of a class or shared resource across an application, accessible everywhere."
- Examples: DB connection, logging service, Java `Runtime` object.
- Naive solution: store instance in a global variable and let all modules read it.
- Problem: any module can overwrite the global variable, breaking the guarantee that everyone uses the same consistent instance.
- Need: shared access without allowing arbitrary modification or re-instantiation.

---

## 2. Core Singleton structure

**Cues / Questions (left)**

- How does Singleton enforce one instance?
- What are the key class members?
- How is access provided?

**Notes (right)**

- Singleton class has:
    - One private static instance field (the sole instance).
    - Private constructor (no external `new`).
    - Public static `getInstance()` method acting as a getter.
- App modules never call `new`; they call `Singleton.getInstance()` to get the global instance.
- Because constructor is private and instance field is private, no one can directly create or replace the instance.

---

## 3. Prime Minister calendar analogy

- The country has one Prime Minister with one official calendar (the shared resource).
- Wrong design: give each person direct access — they can override entries; calendars diverge.
- Correct design: PM's personal assistant (PA) controls all access.
- PA = `getInstance()`; PM's calendar = singleton resource.

---

## 4. Eager vs lazy loading

| | Eager | Lazy |
|---|---|---|
| **When created** | At class load | On first `getInstance()` call |
| **Memory** | Allocated even if unused | Allocated only when needed |
| **Complexity** | Simpler | Needs null-check (+ thread safety) |

---

## 5. Java implementations

### Eager

```java
public class LoggerSingleton {
    private static final LoggerSingleton instance = new LoggerSingleton();
    private LoggerSingleton() {}
    public static LoggerSingleton getInstance() { return instance; }
}
```

### Lazy (not thread-safe)

```java
private static LoggerSingleton instance = null;
public static LoggerSingleton getInstance() {
    if (instance == null) instance = new LoggerSingleton();
    return instance;
}
```

### Double-Checked Locking (thread-safe)

```java
private static volatile LoggerSingleton instance;
public static LoggerSingleton getInstance() {
    if (instance == null) {
        synchronized (LoggerSingleton.class) {
            if (instance == null) instance = new LoggerSingleton();
        }
    }
    return instance;
}
```

`volatile` ensures visibility of the write across threads. Double check avoids locking on every read after init.

### Bill Pugh (recommended)

```java
public class LoggerSingleton {
    private LoggerSingleton() {}
    private static class Holder {
        static final LoggerSingleton INSTANCE = new LoggerSingleton();
    }
    public static LoggerSingleton getInstance() { return Holder.INSTANCE; }
}
```

### Enum (safest)

```java
public enum LoggerSingleton {
    INSTANCE;
    public void log(String msg) { ... }
}
```

---

## 6. Constructor guard against reflection

```java
private LoggerSingleton() {
    if (instance != null) throw new RuntimeException("Use getInstance()");
}
```

---

## 7. Approach comparison

| Approach | When to use |
|----------|------------|
| **Enum** | Default choice — safe against reflection, serialization, and multi-threading with zero extra code |
| **Bill Pugh** | Need lazy init but class must extend a parent (Enums can't) |
| **DCL** | Useful to know for interview (`volatile` / instruction reordering); rarely written in new code |

---

## 8. Pros and cons

- **Pros:** centralised shared resource; guaranteed single instance; easy to reason about
- **Cons:** global state makes unit testing harder (state leaks between tests); can be misused for things that aren't truly global; if parameterised it becomes a factory (Singleton violation)

---

## Related Notes
- [[Design Pattern]] — full GoF index
- [[Computing/Architectonic Pillars/System/Infrastructure/Concurrency|Concurrency]] — thread safety context (`volatile`, `synchronized`)
- [[Computing/Architectonic Pillars/Programming Paradigm & Framework/Java/Concurrency in Java|Concurrency in Java]] — `volatile`, double-checked locking in practice
