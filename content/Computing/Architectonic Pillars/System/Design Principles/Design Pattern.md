# Design Patterns — Index

**Summary**: Covers Design Patterns — Index as a design principle or engineering pattern, with practical implications and examples.
**Tags**: #system-design #design-principles #design-pattern
**Created**: Unknown
**Last Updated**: 2026-05-20

---

GoF patterns organised by category. Each link is a dedicated note; stubs are marked 🔲.

> Full Singleton content moved to [[Patterns/Singleton]] — see that note for implementation, thread safety, and trade-offs.

---

## Creational — *how objects are created*

| Pattern | Intent | Note |
|---------|--------|------|
| **Singleton** | Ensure one instance; global access point | [[Patterns/Singleton]] |
| **Factory Method** | Delegate instantiation to subclasses | 🔲 `Patterns/Factory.md` |
| **Abstract Factory** | Family of related objects without concrete classes | 🔲 `Patterns/AbstractFactory.md` |
| **Builder** | Construct complex objects step-by-step | 🔲 `Patterns/Builder.md` |
| **Prototype** | Clone existing objects | 🔲 `Patterns/Prototype.md` |

---

## Structural — *how objects are composed*

| Pattern | Intent | Note |
|---------|--------|------|
| **Adapter** | Bridge incompatible interfaces | 🔲 `Patterns/Adapter.md` |
| **Decorator** | Add behaviour without modifying class | 🔲 `Patterns/Decorator.md` |
| **Proxy** | Control access to an object | 🔲 `Patterns/Proxy.md` |
| **Facade** | Simplified interface to a subsystem | 🔲 `Patterns/Facade.md` |
| **Composite** | Tree structures of objects | 🔲 `Patterns/Composite.md` |

---

## Behavioural — *how objects communicate*

| Pattern | Intent | Note |
|---------|--------|------|
| **Strategy** | Swap algorithms at runtime | 🔲 `Patterns/Strategy.md` |
| **Observer** | Notify dependents on state change | 🔲 `Patterns/Observer.md` |
| **Command** | Encapsulate requests as objects | 🔲 `Patterns/Command.md` |
| **Template Method** | Skeleton algorithm; subclasses fill steps | 🔲 `Patterns/TemplateMethod.md` |
| **Iterator** | Sequential access without exposing internals | 🔲 `Patterns/Iterator.md` |
| **Chain of Responsibility** | Pass request along a chain of handlers | 🔲 `Patterns/ChainOfResponsibility.md` |

---

## Where Patterns Appear in This Vault

| Pattern | Where used |
|---------|-----------|
| **Singleton** | [[Concurrency in Java]] — `LoggerSingleton` example |
| **Strategy** | [[Rate Limiter]] — swappable rate-limiting algorithm |
| **Strategy + Factory** | [[notification]] — channel dispatch |
| **Template Method** | Spring `JdbcTemplate`, `KafkaTemplate` |
| **Observer** | RxJS / Angular `Observable`; Kafka consumer groups |

---

## Related Notes
- [[00. Overview]] — Design Principles overview
- [[OOPS Design Principles]] — SOLID principles that motivate many patterns
- [[Clean Architecture]] — patterns applied at architectural scale
