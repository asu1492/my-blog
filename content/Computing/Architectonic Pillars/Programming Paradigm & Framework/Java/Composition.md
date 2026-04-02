
1. HAS-A relationship, other related HAS-A relationship - [[Aggregation]]
2. Composition is creating complex object using simpler object. E.g. Order Object is created using OrderItem. 
3. Unlike aggregation, in composition, once container object is destroyed, contained object is also destroyed, that is, they do not have independent existence. 
4. When to Use Composition
	1. We can use it when it does not make sense for an entity to exist independently, e.g. engine is irrelevant without car. E.g. line item cannot exist independently of order, package is meaningless without shipment. 
	2. However, not use, where object can exist independently, e.g. Product can exist independently of Supplier. 
5. How to achieve composition 
	1. When Object is created within the class 
6. Code Example using Order and LineItem Object
	```java
	class LineItem {
	    private String product;
	    private int quantity;
	    
	    public LineItem(String product, int quantity) {
	        this.product = product;
	        this.quantity = quantity;
	        }
	    public void displayItem() {
	        System.out.println("Product: " + product + ", Quantity: " +   
	        quantity);
	    }
	}
	
	class Order {
	    private List<LineItem> items; // Strong Composition - Order "has" LineItems
	    public Order() {
	        this.items = new ArrayList<>(); // LineItems are part of Order's  
	        //lifecycle
	    }
	    public void addItem(String product, int quantity) {
	        items.add(new LineItem(product, quantity)); // Creating LineItem inside Order
	
	    }
	    public void displayOrder() {
	        System.out.println("Order contains:");
	        for (LineItem item : items) {
	            item.displayItem();
	        }
	    }
	}
	
	public class Main {
	    public static void main(String[] args) {
	        Order order = new Order(); // Creating an Order
	        order.addItem("Laptop", 1); // LineItem belongs to Order
	        order.addItem("Mouse", 2);
	        order.displayOrder(); // Displays all items in the order
	        // If Order is deleted, its LineItems are also deleted (no independent existence)
	}	
```
![[Pasted image 20250201175228.png]]