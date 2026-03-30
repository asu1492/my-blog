---
title: Aggregation in Java
date: 2025-03-27
tags:
  - java
  - oop
---

Aggregation is a **HAS-A** relationship in Object-Oriented Programming.

Key characteristics:
1. It is a HAS-A relationship.
2. The child object exists **independently** of the parent object — e.g., a `Customer` object exists independently of an `Order` object.
3. Carried out by passing a reference of the object through a constructor or setter injection.

## Method 1: Constructor Injection

```java
class Customer {
    private String name;

    public Customer(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Order {
    private Customer customer; // Aggregation - Order "has a" Customer

    // Injecting customer object through constructor
    public Order(Customer customer) {
        this.customer = customer;
    }

    public void displayOrderDetails() {
        System.out.println("Order placed by: " + customer.getName());
    }
}

public class Main {
    public static void main(String[] args) {
        Customer customer = new Customer("John Doe"); // Customer exists independently
        Order order = new Order(customer);            // Aggregation: Order "has-a" Customer
        order.displayOrderDetails();                  // Output: Order placed by: John Doe
    }
}
```

## Method 2: Setter Injection

```java
class Order {
    private Customer customer; // Aggregation

    public void setCustomer(Customer customer) {
        this.customer = customer;
    }

    public void displayOrderDetails() {
        if (customer != null) {
            System.out.println("Order placed by: " + customer.getName());
        } else {
            System.out.println("No customer assigned.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Customer customer = new Customer("John Doe");
        Order order = new Order();

        order.setCustomer(customer); // Injecting customer after object creation
        order.displayOrderDetails();
    }
}
```
