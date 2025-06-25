###Problem

Link: https://www.1point3acres.com/bbs/thread-1130360-1-1.html
https://www.1point3acres.com/bbs/thread-1133037-1-1.html

### Level 1: In-Memory Database with Basic Operations
For Level 1, we'll focus on implementing SET, CompareAndSet, and Get.

```java
import java.util.HashMap; // Using HashMap
import java.util.TreeMap; // For Level 4: Equivalent to ConcurrentSkipListMap for single-threaded scenario
import java.util.Map;
import java.util.SortedMap; // To specify return type for SCAN for Level 2

// Represents a single data entry (field-value pair with metadata)
class Record {
    private String value;
    private int timestamp;
    private long expirationTime; // For Level 3: System.currentTimeMillis() + TTL

    public Record(String value, int timestamp) {
        this.value = value;
        this.timestamp = timestamp;
        this.expirationTime = -1; // -1 indicates no TTL for now
    }

    public Record(String value, int timestamp, long expirationTime) {
        this.value = value;
        this.timestamp = timestamp;
        this.expirationTime = expirationTime;
    }

    public String getValue() {
        return value;
    }

    public void setValue(String value) {
        this.value = value;
    }

    public int getTimestamp() {
        return timestamp;
    }

    public void setTimestamp(int timestamp) {
        this.timestamp = timestamp;
    }

    public long getExpirationTime() {
        return expirationTime;
    }

    public void setExpirationTime(long expirationTime) {
        this.expirationTime = expirationTime;
    }

    // For debugging and clear output
    @Override
    public String toString() {
        return "Record{" +
               "value='" + value + '\'' +
               ", timestamp=" + timestamp +
               ", expirationTime=" + expirationTime +
               '}';
    }
}

public class InMemoryDatabase {

    // Structure: key -> (field -> Record)
    private HashMap<String, HashMap<String, Record>> dataStore;

    // For Level 4: key -> (field -> (timestamp -> value))
    private HashMap<String, HashMap<String, TreeMap<Integer, String>>> historicalDataStore;

    public InMemoryDatabase() {
        this.dataStore = new HashMap<>();
        this.historicalDataStore = new HashMap<>(); // Initialize for Level 4 readiness
    }

    // Level 1: SET operation
    public void SET(int timestamp, String key, String field, String value) {
        Record newRecord = new Record(value, timestamp);
        // Using computeIfAbsent for cleaner code and handling new keys/fields
        dataStore.computeIfAbsent(key, k -> new HashMap<>()).put(field, newRecord);

        // Store value for historical look-back (Level 4)
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>()) // TreeMap for sorting by timestamp
                           .put(timestamp, value);
    }

    // Level 1: CompareAndSet operation
    public boolean CompareAndSet(int timestamp, String key, String field, String expectValue, String newValue) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false; // Key does not exist
        }

        Record currentRecord = fields.get(field);
        if (currentRecord == null || !currentRecord.getValue().equals(expectValue)) {
            return false; // Field does not exist or expectValue does not match
        }

        // Replace the record if the value matches expectValue
        Record updatedRecord = new Record(newValue, timestamp);
        // Note: HashMap's replace method (with oldValue) is not atomic for concurrent access
        // but works for single-threaded or external synchronization.
        fields.put(field, updatedRecord); // Simple put overwrites, assumes currentRecord check is enough for logic

        // Store new value for historical look-back (Level 4)
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, newValue);
        return true; // We successfully performed the logical CAS
    }

    // Level 1: Get operation
    public String Get(int time, String key, String field) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return null; // Key does not exist
        }
        Record record = fields.get(field);
        return record != null ? record.getValue() : null;
    }

    // Level 1: CAD (Compare and check if expectValue matches current value)
    public boolean CAD(int timestamp, String key, String field, String expectValue) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false; // Key does not exist
        }
        Record record = fields.get(field);
        return record != null && expectValue.equals(record.getValue());
    }
}
```

### Level 2: Display Records Based on Filters
For Level 2, we'll implement SCAN and ScanByPrefix. Both operations will retrieve field-value pairs for a given key, with ScanByPrefix adding a filter for field names. The results need to be sorted by field in lexicographical order.


```java
import java.util.HashMap;
import java.util.TreeMap; // For Level 4: Equivalent to ConcurrentSkipListMap for single-threaded scenario
import java.util.Map;
import java.util.SortedMap; // To specify return type for SCAN for Level 2
import java.util.stream.Collectors; // For stream operations

// (Record class remains the same as previously defined)

public class InMemoryDatabase {

    // Structure: key -> (field -> Record)
    private HashMap<String, HashMap<String, Record>> dataStore;

    // For Level 4: key -> (field -> (timestamp -> value))
    private HashMap<String, HashMap<String, TreeMap<Integer, String>>> historicalDataStore;

    public InMemoryDatabase() {
        this.dataStore = new HashMap<>();
        this.historicalDataStore = new HashMap<>();
    }

    // Level 1 methods (re-iterated for completeness in this file block):
    public void SET(int timestamp, String key, String field, String value) {
        Record newRecord = new Record(value, timestamp);
        dataStore.computeIfAbsent(key, k -> new HashMap<>()).put(field, newRecord);
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, value);
    }

    public boolean CompareAndSet(int timestamp, String key, String field, String expectValue, String newValue) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false;
        }
        Record currentRecord = fields.get(field);
        if (currentRecord == null || !currentRecord.getValue().equals(expectValue)) {
            return false;
        }
        Record updatedRecord = new Record(newValue, timestamp);
        fields.put(field, updatedRecord);
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, newValue);
        return true;
    }

    public String Get(int time, String key, String field) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return null;
        }
        Record record = fields.get(field);
        return record != null ? record.getValue() : null;
    }

    public boolean CAD(int timestamp, String key, String field, String expectValue) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false;
        }
        Record record = fields.get(field);
        return record != null && expectValue.equals(record.getValue());
    }

    // Level 2: SCAN operation
    public SortedMap<String, String> SCAN(int time, String key) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return new TreeMap<>(); // Return an empty sorted map if key not found
        }

        // Filter out expired records (for Level 3 readiness), then map to field->value, and sort by field
        return fields.entrySet().stream()
                     .filter(entry -> !isExpired(entry.getValue())) // Filter for Level 3
                     .collect(Collectors.toMap(
                         Map.Entry::getKey,
                         entry -> entry.getValue().getValue(),
                         (oldValue, newValue) -> oldValue, // Merge function for duplicates (shouldn't happen with entrySet)
                         TreeMap::new // Ensure the resulting map is sorted by key (field)
                     ));
    }

    // Level 2: ScanByPrefix operation
    public SortedMap<String, String> ScanByPrefix(int time, String key, String prefix) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return new TreeMap<>(); // Return an empty sorted map if key not found
        }

        // Filter by prefix and expiration (Level 3), then map and sort
        return fields.entrySet().stream()
                     .filter(entry -> entry.getKey().startsWith(prefix)) // Filter by prefix
                     .filter(entry -> !isExpired(entry.getValue())) // Filter for Level 3
                     .collect(Collectors.toMap(
                         Map.Entry::getKey,
                         entry -> entry.getValue().getValue(),
                         (oldValue, newValue) -> oldValue,
                         TreeMap::new
                     ));
    }

    // Helper for TTL (for Level 3) - initially returns false
    private boolean isExpired(Record record) {
        return false; // Will be implemented in Level 3
    }
}
```

### Level 3: Support TTL (Time-To-Live)
Now, let's add SetWithTTL and CASWithTTL. These operations will allow us to specify an expiration time for our records. We'll need to modify the Record class and add logic to check for expiration.

```java
// ... (previous imports and Record class)

public class InMemoryDatabase {

    // ... (dataStore and historicalDataStore declarations, unchanged)

    public InMemoryDatabase() {
        this.dataStore = new HashMap<>();
        this.historicalDataStore = new HashMap<>();
    }

    // Level 1 methods (unchanged, but might be called with TTL in mind for future use):
    public void SET(int timestamp, String key, String field, String value) {
        Record newRecord = new Record(value, timestamp);
        dataStore.computeIfAbsent(key, k -> new HashMap<>()).put(field, newRecord);
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, value);
    }

    public boolean CompareAndSet(int timestamp, String key, String field, String expectValue, String newValue) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false;
        }
        Record currentRecord = fields.get(field);
        if (currentRecord == null || !currentRecord.getValue().equals(expectValue)) {
            return false;
        }
        Record updatedRecord = new Record(newValue, timestamp);
        fields.put(field, updatedRecord);
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, newValue);
        return true;
    }

    public String Get(int time, String key, String field) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return null;
        }
        Record record = fields.get(field);
        // Important: Check for expiration here before returning!
        if (record != null && !isExpired(record)) {
            return record.getValue();
        }
        // If expired or not found, return null
        return null;
    }

    public boolean CAD(int timestamp, String key, String field, String expectValue) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false;
        }
        Record record = fields.get(field);
        // Important: Check for expiration here before returning!
        return record != null && !isExpired(record) && expectValue.equals(record.getValue());
    }

    // Level 2 methods (unchanged, relying on updated isExpired):
    public SortedMap<String, String> SCAN(int time, String key) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return new TreeMap<>();
        }
        return fields.entrySet().stream()
                     .filter(entry -> !isExpired(entry.getValue())) // Now this filter actually works
                     .collect(Collectors.toMap(
                         Map.Entry::getKey,
                         entry -> entry.getValue().getValue(),
                         (oldValue, newValue) -> oldValue,
                         TreeMap::new
                     ));
    }

    public SortedMap<String, String> ScanByPrefix(int time, String key, String prefix) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return new TreeMap<>();
        }
        return fields.entrySet().stream()
                     .filter(entry -> entry.getKey().startsWith(prefix))
                     .filter(entry -> !isExpired(entry.getValue())) // Now this filter actually works
                     .collect(Collectors.toMap(
                         Map.Entry::getKey,
                         entry -> entry.getValue().getValue(),
                         (oldValue, newValue) -> oldValue,
                         TreeMap::new
                     ));
    }

    // Level 3: SET with TTL
    public void SetWithTTL(int timestamp, String key, String field, String value, long ttlMillis) {
        long expirationTime = System.currentTimeMillis() + ttlMillis;
        Record newRecord = new Record(value, timestamp, expirationTime);
        dataStore.computeIfAbsent(key, k -> new HashMap<>()).put(field, newRecord);

        // Still record in historical data without TTL info for look-back
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, value);
    }

    // Level 3: CompareAndSet with TTL
    public boolean CASWithTTL(int timestamp, String key, String field, String expectValue, String newValue, long ttlMillis) {
        HashMap<String, Record> fields = dataStore.get(key);
        if (fields == null) {
            return false;
        }

        Record currentRecord = fields.get(field);
        // Check for expiration of current record as well before comparing value
        if (currentRecord == null || isExpired(currentRecord) || !currentRecord.getValue().equals(expectValue)) {
            return false;
        }

        long expirationTime = System.currentTimeMillis() + ttlMillis;
        Record updatedRecord = new Record(newValue, timestamp, expirationTime);
        fields.put(field, updatedRecord); // Overwrite

        // Store new value for historical look-back
        historicalDataStore.computeIfAbsent(key, k -> new HashMap<>())
                           .computeIfAbsent(field, f -> new TreeMap<>())
                           .put(timestamp, newValue);
        return true;
    }

    // Level 3 Helper: Check if a record is expired
    private boolean isExpired(Record record) {
        // If expirationTime is -1, it means no TTL (never expires)
        // Otherwise, check if current time is past expirationTime
        return record.getExpirationTime() != -1 && System.currentTimeMillis() > record.getExpirationTime();
    }
}
```

### Level 4: Support Look-Back Operation
Finally, Level 4: GetWhen(int timestamp, String key, String field, int timeAt). This requires us to use the historicalDataStore we've been populating.

```java
// ... (previous imports, Record class, and InMemoryDatabase class up to Level 3 methods)

public class InMemoryDatabase {

    // ... (dataStore and historicalDataStore declarations, all Level 1, 2, 3 methods unchanged)

    // Level 4: GetWhen operation
    public String GetWhen(int timestamp, String key, String field, int timeAt) {
        HashMap<String, TreeMap<Integer, String>> keyHistory = historicalDataStore.get(key);
        if (keyHistory == null) {
            return null; // Key does not exist in history
        }

        TreeMap<Integer, String> fieldHistory = keyHistory.get(field);
        if (fieldHistory == null) {
            return null; // Field does not exist in history for this key
        }

        // The TreeMap stores entries sorted by timestamp.
        // floorEntry(timeAt) returns the entry with the greatest key (timestamp)
        // less than or equal to the given timeAt. This is exactly what we need
        // for a "look-back" operation – the value that was current AT or BEFORE timeAt.
        Map.Entry<Integer, String> entry = fieldHistory.floorEntry(timeAt);

        // If an entry is found, return its value
        return entry != null ? entry.getValue() : null;
    }
}
```