###Link
https://www.1point3acres.com/bbs/thread-1131169-1-1.html
https://www.1point3acres.com/bbs/thread-1127184-1-1.html

# Food Delivery System – Levels 1 to 4

A multi-level Java design problem based on optimizing delivery costs, delivery times, and analyzing orders over time.

---

## Level 1 – Basic Delivery Cost and Time Calculation

### Problem

You're given a user location and a list of restaurants. Each restaurant has:

- A location `(x, y)`
- An average delivery speed
- A list of items with their prices

**Tasks:**

1. Calculate the **minimum delivery cost** (item price + distance to user)  
2. Calculate the **shortest delivery time** (distance / speed)  
 
Distance is Euclidean:  
distance = sqrt((x1 - x2)^2 + (y1 - y2)^2)  


---

### Solution

```java
import java.util.*;

class Point {
    double x, y;
    Point(double x, double y) { this.x = x; this.y = y; }
    double distanceTo(Point other) {
        return Math.sqrt(Math.pow(this.x - other.x, 2) + Math.pow(this.y - other.y, 2));
    }
}

class Item {
    String name;
    double price;
    Item(String name, double price) {
        this.name = name;
        this.price = price;
    }
}

class Restaurant {
    int id;
    Point location;
    double avgSpeed;
    List<Item> items;
    Restaurant(int id, Point location, double avgSpeed) {
        this.id = id;
        this.location = location;
        this.avgSpeed = avgSpeed;
        this.items = new ArrayList<>();
    }
}

class DeliverySystemLevel1 {
    Point userLocation;
    List<Restaurant> restaurants;

    DeliverySystemLevel1(Point userLocation, List<Restaurant> restaurants) {
        this.userLocation = userLocation;
        this.restaurants = restaurants;
    }

    double getMinDeliveryCost() {
        double minCost = Double.MAX_VALUE;
        for (Restaurant r : restaurants) {
            double dist = r.location.distanceTo(userLocation);
            for (Item item : r.items) {
                double cost = item.price + dist;
                minCost = Math.min(minCost, cost);
            }
        }
        return minCost;
    }

    double getMinDeliveryTime() {
        double minTime = Double.MAX_VALUE;
        for (Restaurant r : restaurants) {
            double dist = r.location.distanceTo(userLocation);
            if (r.avgSpeed > 0) {
                double time = dist / r.avgSpeed;
                minTime = Math.min(minTime, time);
            }
        }
        return minTime;
    }
}
```


## Level 2 – Order Statistics by Time Range
### Problem
Given a list of orders:  
orderId, itemId, totalPrice, timestamp  
And a time range [start, end], calculate:  
* Number of orders in the range  
* Total revenue in the range  
* Average order value in the range  

### Solution
```java
class Order {
    int orderId;
    int itemId;
    double totalPrice;
    int timestamp;
    Order(int orderId, int itemId, double totalPrice, int timestamp) {
        this.orderId = orderId;
        this.itemId = itemId;
        this.totalPrice = totalPrice;
        this.timestamp = timestamp;
    }
}

class OrderSystem {
    List<Order> orders;

    OrderSystem(List<Order> orders) {
        this.orders = orders;
    }

    int countOrdersInRange(int start, int end) {
        return (int) orders.stream().filter(o -> o.timestamp >= start && o.timestamp <= end).count();
    }

    double totalRevenueInRange(int start, int end) {
        return orders.stream()
            .filter(o -> o.timestamp >= start && o.timestamp <= end)
            .mapToDouble(o -> o.totalPrice)
            .sum();
    }

    double averageOrderValueInRange(int start, int end) {
        double total = totalRevenueInRange(start, end);
        int count = countOrdersInRange(start, end);
        return count == 0 ? 0 : total / count;
    }
}
```

## Level 3 – Item-specific Cost Query
### Problem
Add a method:  
find_min_cost_item(String itemName) → returns minimum cost for that specific item  

### Solution
```java
class DeliverySystemLevel3 extends DeliverySystemLevel1 {

    DeliverySystemLevel3(Point userLocation, List<Restaurant> restaurants) {
        super(userLocation, restaurants);
    }

    double findMinCostItem(String itemName) {
        double minCost = Double.MAX_VALUE;
        for (Restaurant r : restaurants) {
            double dist = r.location.distanceTo(userLocation);
            for (Item item : r.items) {
                if (item.name.equals(itemName)) {
                    double cost = item.price + dist;
                    minCost = Math.min(minCost, cost);
                }
            }
        }
        return minCost == Double.MAX_VALUE ? -1 : minCost;
    }
}
```

## Level 4 – Unified System
### Problem
Create a unified FoodDeliverySystem class that:
* Calculates minimum delivery cost and time
* Returns item-specific min delivery cost
* Performs time-range order analytics (count, total, average)

```java
class FoodDeliverySystem {
    DeliverySystemLevel3 deliverySystem;
    OrderSystem orderSystem;

    FoodDeliverySystem(Point userLocation, List<Restaurant> restaurants, List<Order> orders) {
        this.deliverySystem = new DeliverySystemLevel3(userLocation, restaurants);
        this.orderSystem = new OrderSystem(orders);
    }

    // Delivery-related methods
    double getMinDeliveryCost() {
        return deliverySystem.getMinDeliveryCost();
    }

    double getMinDeliveryTime() {
        return deliverySystem.getMinDeliveryTime();
    }

    double findMinCostItem(String itemName) {
        return deliverySystem.findMinCostItem(itemName);
    }

    // Order analytics
    int countOrdersInRange(int start, int end) {
        return orderSystem.countOrdersInRange(start, end);
    }

    double totalRevenueInRange(int start, int end) {
        return orderSystem.totalRevenueInRange(start, end);
    }

    double averageOrderValueInRange(int start, int end) {
        return orderSystem.averageOrderValueInRange(start, end);
    }
}

```


