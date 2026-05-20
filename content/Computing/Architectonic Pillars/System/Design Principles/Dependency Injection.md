# Dependency Injection

**Summary**: Covers Dependency Injection as a design principle or engineering pattern, with practical implications and examples.
**Tags**: #system-design #design-principles #dependency-injection
**Created**: Unknown
**Last Updated**: 2026-05-20

---

Dependency Injection is a **technique** for implementing the [[Dependency Inversion Principle (DIP)]]. Rather than a class creating its own dependencies, they are *provided* (injected) from outside — making the class depend on an abstraction, not a concrete implementation.

---

## The Three Forms

| Type | How | Example |
|------|-----|---------|
| **Constructor injection** | Dependency passed via constructor | `new OrderService(paymentRepo)` |
| **Setter injection** | Dependency set via a setter method | `service.setRepo(paymentRepo)` |
| **Field injection** | Framework injects directly into field | `@Autowired` in Spring |

Constructor injection is preferred — it makes dependencies explicit and supports immutability.

---

## DIP → DI → Framework

```
DIP (principle)
  "Depend on abstractions, not concretions"
        ↓
DI (technique)
  "Supply the abstraction from outside the class"
        ↓
DI Container (Spring / Angular)
  "A framework that manages wiring automatically"
```

---

## In Spring (Java)

Spring's IoC container manages bean creation and injection. See [[Interface-Based Injection]] for the Spring-specific pattern (`@Autowired`, `@Primary`, `@Qualifier`).

```java
@Service
public class OrderService {
    private final PaymentGateway gateway;

    public OrderService(PaymentGateway gateway) {
        this.gateway = gateway;
    }
}
```

---

## In Angular (TypeScript)

Angular's injector hierarchy mirrors Spring's container. A service is provided at root, module, or component scope.

```typescript
@Injectable({ providedIn: 'root' })
export class AuthService { ... }

@Component({ ... })
export class LoginComponent {
    constructor(private auth: AuthService) {}
}
```

---

## Related Notes
1. [[Dependency Inversion Principle (DIP)]] — the SOLID principle this technique implements
2. [[Interface-Based Injection]] — Spring-specific wiring (`@Autowired`, `@Qualifier`)
3. [[Computing/Architectonic Pillars/Programming Paradigm & Framework/Angular/Angular MOC|Angular MOC]] — Angular DI scope and injector hierarchy
4. [[OOPS Design Principles]] — full SOLID reference
