### Link
https://www.1point3acres.com/bbs/thread-1127184-1-1.html

### Questions

```
Input list of Events: {order_id, order_type, currency, amount, price, event_type}
[
	{1234, BUY, BTC, 20, 100.0, NEW},
	{1234, BUY, BTC, 15, 100.0, FILLED},
	{1234, BUY, BTC, 5, 100.0, FILLED},
]
```
给了很多没用的field，其实真正要你focus的是order_id, amount和event_type

1. Level 1: 输入一个order_id, 返回最后这个order 最后的status。 如果这个order没有被filled，就返回NEW，如果这个order partially filled，就返回IN_PROCESS。如果完全被filled了，返回COMPLETED。
很简单，建立了一个新的Order class，然后一个map搞定。然后问了如果message有重复怎么办，答案idempotency。然后让我point out在现有代码里怎么加这个逻辑。  

2. Level 2: 现在加了一个新的event_type: CANCEL。 只有没有被filled过的order能被cancel。
也是很简单，检查一下order status，然后更新status就行了  

3. Level 3: 没啥时间了，而且reqirement不是很clear，她说没有很多人能做到这一步。大概意思是现在我们想要aggregate这些event，我们需要process event in 5 second window，而且event could come in out of order，我们需要process events in increasing timestamp order。比如说输入timesamp 10，我们就process 6-10的event。  
我说了一下大概思路，就是我们现在不preprocess event了，我们就把event用一个time_to_events的map 存着。然后建个新的function可以接收一个timestamp，然后用map拿events then process。  
大概写了一下代码，没有时间test，也没有时间handle edge case。面试官说他其实想看到一个queue based solution，但这个应该没问题。  

#### Level 1
Level 1: Order Status Tracking
Question: Given a list of order events, determine the final status of a specific order. If an order has not been filled at all, its status is NEW. If it's partially filled, its status is IN_PROCESS. If it's completely filled, its status is COMPLETED.

Input Example:
```
order_id, order_type, currency, amount, price, event_type
{1234, BUY, BTC, 20, 100.0, NEW},
{1234, BUY, BTC, 15, 100.0, FILLED},
{1234, BUY, BTC, 5, 100.0, FILLED},
```
For order_id 1234, the initial amount is 20. It then has two FILLED events for 15 and 5, totaling 20. Thus, the order is COMPLETED.

```java
// Event.java
public class Event {
    int orderId;
    String orderType; // Not directly used in Level 1 status, but part of input
    String currency;  // Not directly used in Level 1 status
    double amount;
    double price;     // Not directly used in Level 1 status
    EventType eventType;

    public Event(int orderId, String orderType, String currency, double amount, double price, EventType eventType) {
        this.orderId = orderId;
        this.orderType = orderType;
        this.currency = currency;
        this.amount = amount;
        this.price = price;
        this.eventType = eventType;
    }

    // Getters for all fields
    public int getOrderId() {
        return orderId;
    }

    public double getAmount() {
        return amount;
    }

    public EventType getEventType() {
        return eventType;
    }
}

// EventType.java
public enum EventType {
    NEW,
    FILLED,
    // CANCEL will be added in Level 2
}

// OrderStatus.java
public enum OrderStatus {
    NEW,
    IN_PROCESS,
    COMPLETED,
    // CANCELED will be added in Level 2
}

// Order.java
public class Order {
    private int orderId;
    private double initialAmount;
    private double filledAmount;
    private OrderStatus status;

    public Order(int orderId, double initialAmount) {
        this.orderId = orderId;
        this.initialAmount = initialAmount;
        this.filledAmount = 0.0;
        this.status = OrderStatus.NEW; // Initial status is NEW
    }

    public int getOrderId() {
        return orderId;
    }

    public double getInitialAmount() {
        return initialAmount;
    }

    public double getFilledAmount() {
        return filledAmount;
    }

    public void addFilledAmount(double amount) {
        this.filledAmount += amount;
        updateStatus(); // Update status whenever filled amount changes
    }

    public OrderStatus getStatus() {
        return status;
    }

    // This method updates the status based on current filledAmount
    private void updateStatus() {
        if (filledAmount == 0) {
            this.status = OrderStatus.NEW;
        } else if (filledAmount < initialAmount) {
            this.status = OrderStatus.IN_PROCESS;
        } else {
            this.status = OrderStatus.COMPLETED;
        }
    }
}

public class OrderProcessor {
    // Map to store current state of orders, keyed by orderId
    private Map<Integer, Order> orders;

    public OrderProcessor() {
        this.orders = new HashMap<>();
    }

    // Processes a single event and updates the order's state
    public void processEvent(Event event) {
        int orderId = event.getOrderId();
        EventType eventType = event.getEventType();
        double eventAmount = event.getAmount();

        switch (eventType) {
            case NEW:
                // If a NEW event comes for an existing order, it might be a duplicate
                // For simplicity in Level 1, we assume NEW event initializes the order.
                // Idempotency: We could check if the order already exists.
                // If orders.containsKey(orderId), we might ignore this NEW event
                // or validate if the initialAmount is consistent.
                if (!orders.containsKey(orderId)) {
                    orders.put(orderId, new Order(orderId, eventAmount));
                }
                break;
            case FILLED:
                if (orders.containsKey(orderId)) {
                    Order order = orders.get(orderId);
                    // Ensure we don't overfill the order
                    double amountToFill = Math.min(eventAmount, order.getInitialAmount() - order.getFilledAmount());
                    if (amountToFill > 0) {
                        order.addFilledAmount(amountToFill);
                    }
                } else {
                    // This could be an out-of-order FILLED event before a NEW event, or an error.
                    // For Level 1, we assume events arrive in a reasonable order or NEW event comes first.
                    System.out.println("Warning: FILLED event for unknown orderId: " + orderId);
                }
                break;
            default:
                System.out.println("Unknown event type: " + eventType);
                break;
        }
    }

    // Returns the final status of a given orderId
    public OrderStatus getOrderStatus(int orderId) {
        Order order = orders.get(orderId);
        if (order == null) {
            return null; // Order not found
        }
        return order.getStatus();
    }

    // Helper to process a list of events
    public void processEvents(List<Event> events) {
        for (Event event : events) {
            processEvent(event);
        }
    }
}
```

Test case:
```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        OrderProcessor processor = new OrderProcessor();

        // Level 1 Example 1: Completed Order
        List<Event> events1 = new ArrayList<>();
        events1.add(new Event(1234, "BUY", "BTC", 20, 100.0, EventType.NEW));
        events1.add(new Event(1234, "BUY", "BTC", 15, 100.0, EventType.FILLED));
        events1.add(new Event(1234, "BUY", "BTC", 5, 100.0, EventType.FILLED));
        processor.processEvents(events1);
        System.out.println("Order 1234 Status: " + processor.getOrderStatus(1234)); // Expected: COMPLETED
        System.out.println("---");

        // Level 1 Example 2: Partially Filled Order
        List<Event> events2 = new ArrayList<>();
        events2.add(new Event(5678, "SELL", "ETH", 30, 2000.0, EventType.NEW));
        events2.add(new Event(5678, "SELL", "ETH", 10, 2000.0, EventType.FILLED));
        processor.processEvents(events2);
        System.out.println("Order 5678 Status: " + processor.getOrderStatus(5678)); // Expected: IN_PROCESS
        System.out.println("---");

        // Level 1 Example 3: New Order (not filled)
        List<Event> events3 = new ArrayList<>();
        events3.add(new Event(9012, "BUY", "LTC", 5, 50.0, EventType.NEW));
        processor.processEvents(events3);
        System.out.println("Order 9012 Status: " + processor.getOrderStatus(9012)); // Expected: NEW
        System.out.println("---");

        // Example: Order Not Found
        System.out.println("Order 9999 Status: " + processor.getOrderStatus(9999)); // Expected: null
    }
}
```

#### Idempotency Discussion
In our current processEvent for Level 1:

NEW Event: The line if (!orders.containsKey(orderId)) helps with idempotency for NEW events. If a NEW event for an orderId comes in again, and that orderId is already in our orders map, we simply ignore it or can add a check to ensure the initialAmount is consistent. This prevents re-initializing an order that's already being processed.

FILLED Event: For FILLED events, they are inherently cumulative. If we receive the same FILLED event (e.g., {1234, FILLED, 5}) twice, our current code would add 5 to filledAmount twice, leading to an incorrect total. To make FILLED events idempotent, each FILLED event would need a unique identifier (e.g., a transactionId or eventId). We would then store a set of processed transactionIds for each order to prevent processing the same fill event multiple times.

Where to add idempotency logic for FILLED events in existing code:  
To add idempotency for FILLED events, we'd modify the Event class and the processEvent method.

```java
public class Event {
    // ... existing fields ...
    String eventUniqueId; // A unique identifier for this specific event message

    public Event(int orderId, String orderType, String currency, double amount, double price, EventType eventType, String eventUniqueId) {
        // ... constructor assignments ...
        this.eventUniqueId = eventUniqueId;
    }

    public String getEventUniqueId() {
        return eventUniqueId;
    }
}
```

```java
import java.util.HashSet;
import java.util.Set;

public class Order {
    // ... existing fields ...
    private Set<String> processedEventIds; // To track unique FILLED event IDs

    public Order(int orderId, double initialAmount) {
        // ... existing constructor assignments ...
        this.processedEventIds = new HashSet<>();
    }

    public boolean hasProcessedEvent(String eventId) {
        return processedEventIds.contains(eventId);
    }

    public void addProcessedEventId(String eventId) {
        processedEventIds.add(eventId);
    }

    // No change to addFilledAmount, it's called after the idempotency check
    // ... existing addFilledAmount ...
}
```

#### Level 2: Adding Cancellation
Question: Now, add a new EventType: CANCEL. An order can only be canceled if it has not been completely filled. If a CANCEL event is received for an order that is NEW or IN_PROCESS, its status should change to CANCELED. If it's already COMPLETED, the CANCEL event should be ignored.


Solution。
1. Update EventType.java:
```java
public enum EventType {
    NEW,
    FILLED,
    CANCEL // Added CANCEL event type
}
```

2. Update OrderStatus.java:
```java
public enum OrderStatus {
    NEW,
    IN_PROCESS,
    COMPLETED,
    CANCELED // Added CANCELED status
}
```

3. Update Order.java:
We'll add a method to allow setting the status to CANCELED. We'll also make sure updateStatus() doesn't override CANCELED if the order is already canceled.
```java
// Order.java (modified)
public class Order {
    private int orderId;
    private double initialAmount;
    private double filledAmount;
    private OrderStatus status;

    public Order(int orderId, double initialAmount) {
        this.orderId = orderId;
        this.initialAmount = initialAmount;
        this.filledAmount = 0.0;
        this.status = OrderStatus.NEW;
    }

    // ... existing getters ...

    public void addFilledAmount(double amount) {
        if (this.status == OrderStatus.CANCELED || this.status == OrderStatus.COMPLETED) {
            // Do not add filled amount if already canceled or completed
            return;
        }
        this.filledAmount += amount;
        updateStatus();
    }

    public void cancelOrder() {
        // An order can only be canceled if it's NEW or IN_PROCESS
        if (this.status == OrderStatus.NEW || this.status == OrderStatus.IN_PROCESS) {
            this.status = OrderStatus.CANCELED;
        } else {
            // If it's already COMPLETED, or already CANCELED, do nothing
            System.out.println("Warning: Attempted to cancel order " + orderId + " which is already " + this.status);
        }
    }

    public OrderStatus getStatus() {
        return status;
    }

    // Modified updateStatus to respect CANCELED state
    private void updateStatus() {
        if (this.status == OrderStatus.CANCELED) {
            return; // If already canceled, don't change status based on fill
        }
        if (filledAmount == 0) {
            this.status = OrderStatus.NEW;
        } else if (filledAmount < initialAmount) {
            this.status = OrderStatus.IN_PROCESS;
        } else { // filledAmount >= initialAmount
            this.status = OrderStatus.COMPLETED;
        }
    }
}
```

4. Update OrderProcessor.java:  
We'll add a case for CANCEL events.
```java
// OrderProcessor.java (modified)
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class OrderProcessor {
    private Map<Integer, Order> orders;

    public OrderProcessor() {
        this.orders = new HashMap<>();
    }

    public void processEvent(Event event) {
        int orderId = event.getOrderId();
        EventType eventType = event.getEventType();
        double eventAmount = event.getAmount();

        switch (eventType) {
            case NEW:
                if (!orders.containsKey(orderId)) {
                    orders.put(orderId, new Order(orderId, eventAmount));
                }
                break;
            case FILLED:
                if (orders.containsKey(orderId)) {
                    Order order = orders.get(orderId);
                    // Ensure we don't try to fill a canceled or completed order
                    if (order.getStatus() != OrderStatus.CANCELED && order.getStatus() != OrderStatus.COMPLETED) {
                        double amountToFill = Math.min(eventAmount, order.getInitialAmount() - order.getFilledAmount());
                        if (amountToFill > 0) {
                            order.addFilledAmount(amountToFill);
                        }
                    } else {
                        System.out.println("Warning: Ignoring FILLED event for order " + orderId + " which is " + order.getStatus());
                    }
                } else {
                    System.out.println("Warning: FILLED event for unknown orderId: " + orderId);
                }
                break;
            case CANCEL: // New case for CANCEL events
                if (orders.containsKey(orderId)) {
                    Order order = orders.get(orderId);
                    // Logic: Only cancel if not already COMPLETED
                    if (order.getStatus() != OrderStatus.COMPLETED) {
                        order.cancelOrder(); // The Order class handles the status change logic
                    } else {
                        System.out.println("Warning: Cannot cancel COMPLETED order " + orderId);
                    }
                } else {
                    System.out.println("Warning: CANCEL event for unknown orderId: " + orderId);
                }
                break;
            default:
                System.out.println("Unknown event type: " + eventType);
                break;
        }
    }

    // ... getOrderStatus and processEvents methods remain the same ...
}
```

Test case for Level 2
```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        OrderProcessor processor = new OrderProcessor();

        // Level 2 Example 1: Successful Cancellation
        List<Event> events1 = new ArrayList<>();
        events1.add(new Event(1111, "BUY", "BTC", 20, 100.0, EventType.NEW));
        events1.add(new Event(1111, "BUY", "BTC", 5, 100.0, EventType.FILLED)); // Partially filled
        events1.add(new Event(1111, "BUY", "BTC", 0, 0.0, EventType.CANCEL)); // Amount/price often irrelevant for CANCEL
        processor.processEvents(events1);
        System.out.println("Order 1111 Status: " + processor.getOrderStatus(1111)); // Expected: CANCELED
        System.out.println("---");

        // Level 2 Example 2: Cancellation of a NEW order
        List<Event> events2 = new ArrayList<>();
        events2.add(new Event(2222, "SELL", "ETH", 10, 2000.0, EventType.NEW));
        events2.add(new Event(2222, "SELL", "ETH", 0, 0.0, EventType.CANCEL));
        processor.processEvents(events2);
        System.out.println("Order 2222 Status: " + processor.getOrderStatus(2222)); // Expected: CANCELED
        System.out.println("---");

        // Level 2 Example 3: Attempt to cancel a COMPLETED order (should be ignored)
        List<Event> events3 = new ArrayList<>();
        events3.add(new Event(3333, "BUY", "LTC", 10, 50.0, EventType.NEW));
        events3.add(new Event(3333, "BUY", "LTC", 10, 50.0, EventType.FILLED)); // Completed
        events3.add(new Event(3333, "BUY", "LTC", 0, 0.0, EventType.CANCEL));
        processor.processEvents(events3);
        System.out.println("Order 3333 Status: " + processor.getOrderStatus(3333)); // Expected: COMPLETED
        System.out.println("---");

        // Level 2 Example 4: Filling a CANCELED order (should be ignored)
        List<Event> events4 = new ArrayList<>();
        events4.add(new Event(4444, "BUY", "XYZ", 100, 10.0, EventType.NEW));
        events4.add(new Event(4444, "BUY", "XYZ", 0, 0.0, EventType.CANCEL)); // Canceled
        events4.add(new Event(4444, "BUY", "XYZ", 50, 10.0, EventType.FILLED)); // Should be ignored
        processor.processEvents(events4);
        System.out.println("Order 4444 Status: " + processor.getOrderStatus(4444)); // Expected: CANCELED
        System.out.println("---");
    }
}
```

#### Level 3: Windowed Event Processing and Out-of-Order Events
Question: We need to aggregate these events, processing them in a 5-second window. Events can come in out of order, so we need to process events in increasing timestamp order within that window. For example, if the input timestamp is 10, we process events from timestamp 6-10.

1. Update Event.java: Add a timestamp field. We'll also make it Comparable so it can be easily sorted in collections (though TreeMap sorts by key, this is good practice for events).
```java
// Event.java (modified for Level 3)
public class Event implements Comparable<Event> {
    int orderId;
    String orderType;
    String currency;
    double amount;
    double price;
    EventType eventType;
    long timestamp; // New field: Unix timestamp in milliseconds

    public Event(int orderId, String orderType, String currency, double amount, double price, EventType eventType, long timestamp) {
        this.orderId = orderId;
        this.orderType = orderType;
        this.currency = currency;
        this.amount = amount;
        this.price = price;
        this.eventType = eventType;
        this.timestamp = timestamp;
    }

    // ... existing getters ...

    public long getTimestamp() {
        return timestamp;
    }

    @Override
    public int compareTo(Event other) {
        return Long.compare(this.timestamp, other.timestamp);
    }

    @Override
    public String toString() {
        return "Event{" +
               "orderId=" + orderId +
               ", eventType=" + eventType +
               ", amount=" + amount +
               ", timestamp=" + timestamp +
               '}';
    }
}
```

2. New WindowedOrderProcessor (or modify OrderProcessor):
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;
import java.util.Iterator; // For safe removal during iteration

public class WindowedOrderProcessor {
    private OrderProcessor orderProcessor; // Our Level 2 processor
    // TreeMap to store events, keyed by timestamp for sorted access
    // Value is a list because multiple events can happen at the same timestamp
    private TreeMap<Long, List<Event>> eventBuffer;
    private final long WINDOW_SIZE_MS = 5000; // 5 seconds in milliseconds

    public WindowedOrderProcessor() {
        this.orderProcessor = new OrderProcessor();
        this.eventBuffer = new TreeMap<>();
    }

    // Method to ingest new events (they go into the buffer)
    public void ingestEvent(Event event) {
        eventBuffer.computeIfAbsent(event.getTimestamp(), k -> new ArrayList<>()).add(event);
        System.out.println("Ingested event: " + event);
    }

    // Processes all events within the specified time window, up to currentTimestamp
    public void processWindow(long currentTimestamp) {
        System.out.println("\nProcessing window up to timestamp: " + currentTimestamp);

        long windowStart = currentTimestamp - WINDOW_SIZE_MS;

        // Get a sub-map of events within the current window
        // headMap(key, true) includes key, tailMap(key, true) includes key
        // subMap(fromKey, fromInclusive, toKey, toInclusive)
        Map<Long, List<Event>> eventsToProcess = eventBuffer.subMap(windowStart, true, currentTimestamp, true);

        // Create a temporary list to hold events for processing to avoid ConcurrentModificationException
        List<Event> orderedEventsInWindow = new ArrayList<>();
        for (Map.Entry<Long, List<Event>> entry : eventsToProcess.entrySet()) {
            orderedEventsInWindow.addAll(entry.getValue());
        }

        // Sort within the window if there were events with same timestamp but different order
        // (Our current Event.java implements Comparable, but TreeMap ensures order by timestamp key)
        // If events within the same timestamp need specific ordering (e.g., NEW before FILLED),
        // you'd add more complex sorting here. For now, TreeMap ensures ordering by timestamp.

        // Process events in timestamp order
        for (Event event : orderedEventsInWindow) {
            System.out.println("  Processing event: " + event);
            orderProcessor.processEvent(event);
        }

        // Remove processed events from the buffer
        // Use an iterator to safely remove elements while iterating
        Iterator<Map.Entry<Long, List<Event>>> iterator = eventBuffer.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<Long, List<Event>> entry = iterator.next();
            if (entry.getKey() <= currentTimestamp) { // Remove all events up to and including currentTimestamp
                iterator.remove();
            } else {
                // Since TreeMap is sorted, once we hit an event outside the window,
                // all subsequent events are also outside.
                break;
            }
        }
        System.out.println("Finished processing window. Remaining events in buffer: " + eventBuffer.size());
    }

    // Returns the status from the underlying OrderProcessor
    public OrderStatus getOrderStatus(int orderId) {
        return orderProcessor.getOrderStatus(orderId);
    }
}
```

How to Implement:  
* Keep Level 1 & 2 files: Make sure you have Event.java (modified with timestamp), EventType.java, OrderStatus.java, Order.java, and OrderProcessor.java in your project.
* Create WindowedOrderProcessor.java: Add this new file.
* Use the WindowedOrderProcessor:

Test case:
```java
import java.util.concurrent.TimeUnit; // For simulating time progression

public class Main {
    public static void main(String[] args) throws InterruptedException {
        WindowedOrderProcessor windowProcessor = new WindowedOrderProcessor();

        // Simulate events arriving out of order and at different times

        // Time 1000ms
        windowProcessor.ingestEvent(new Event(100, "BUY", "BTC", 10, 100.0, EventType.NEW, 1000));
        windowProcessor.ingestEvent(new Event(200, "SELL", "ETH", 5, 2000.0, EventType.NEW, 1000));

        // Time 2000ms - an early fill for order 100, and a new order 300
        windowProcessor.ingestEvent(new Event(100, "BUY", "BTC", 3, 100.0, EventType.FILLED, 2000));
        windowProcessor.ingestEvent(new Event(300, "BUY", "LTC", 20, 50.0, EventType.NEW, 2000));

        // Time 4000ms - a cancellation, and another fill for order 100
        windowProcessor.ingestEvent(new Event(300, "BUY", "LTC", 0, 0.0, EventType.CANCEL, 4000));
        windowProcessor.ingestEvent(new Event(100, "BUY", "BTC", 2, 100.0, EventType.FILLED, 4000));

        // Time 6000ms - a late event for order 100, and a fill for order 200
        windowProcessor.ingestEvent(new Event(100, "BUY", "BTC", 5, 100.0, EventType.FILLED, 6000)); // Will complete order 100 now (3+2+5=10)
        windowProcessor.ingestEvent(new Event(200, "SELL", "ETH", 2, 2000.0, EventType.FILLED, 6000));

        // Time 5000ms - an event that arrived "late" but has an earlier timestamp
        windowProcessor.ingestEvent(new Event(100, "BUY", "BTC", 0, 0.0, EventType.CANCEL, 500)); // Should be ignored because order 100 gets filled later
        windowProcessor.ingestEvent(new Event(400, "SELL", "DOGE", 100, 0.5, EventType.NEW, 5000));


        // --- Simulation of time progressing and processing windows ---

        // Let's simulate processing at different time points
        // First window at 4500ms: should process events from 0-4500
        TimeUnit.MILLISECONDS.sleep(100); // Simulate a small delay
        windowProcessor.processWindow(4500); // Processes events up to 4500ms

        System.out.println("Order 100 Status after 4500ms window: " + windowProcessor.getOrderStatus(100)); // Expected: IN_PROCESS (3+2=5 filled, initial 10)
        System.out.println("Order 200 Status after 4500ms window: " + windowProcessor.getOrderStatus(200)); // Expected: NEW
        System.out.println("Order 300 Status after 4500ms window: " + windowProcessor.getOrderStatus(300)); // Expected: CANCELED
        System.out.println("Order 400 Status after 4500ms window: " + windowProcessor.getOrderStatus(400)); // Expected: NEW
        System.out.println("---");


        // Next window at 7000ms: should process events from 4501-7000 (specifically, 5000, 6000)
        TimeUnit.MILLISECONDS.sleep(100);
        windowProcessor.processWindow(7000);

        System.out.println("Order 100 Status after 7000ms window: " + windowProcessor.getOrderStatus(100)); // Expected: COMPLETED (5+5=10 filled)
        System.out.println("Order 200 Status after 7000ms window: " + windowProcessor.getOrderStatus(200)); // Expected: IN_PROCESS (2 filled)
        System.out.println("Order 400 Status after 7000ms window: " + windowProcessor.getOrderStatus(400)); // Expected: NEW
        System.out.println("---");

        // What if we try to cancel order 100 now, but it was already completed in the previous window?
        windowProcessor.ingestEvent(new Event(100, "BUY", "BTC", 0, 0.0, EventType.CANCEL, 8000));
        TimeUnit.MILLISECONDS.sleep(100);
        windowProcessor.processWindow(8000);
        System.out.println("Order 100 Status after 8000ms window: " + windowProcessor.getOrderStatus(100)); // Expected: COMPLETED (cancel ignored)
        System.out.println("---");
    }
}
```
