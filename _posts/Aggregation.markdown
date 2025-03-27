---
layout: post
title:  "Aggregation in Java"
date:   2025-03-27 22:29:52 +0530
categories: java
---


1. Is a HAS-A relationship 
2. child object exist independently of parent object, e.g. Customer Object exist independently of Order Object. 
3. Carried out passing reference of the object through constructor or by using setter injection.
	1. Passing Reference of Customer Object to Order Constructor 
		``` java
		
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
		        Order order = new Order(customer); // Aggregation: Order "has-a" Customer
		        order.displayOrderDetails(); // Output: Order placed by: John Doe
		    }
		}
		```
	2. Using Setter Injection
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