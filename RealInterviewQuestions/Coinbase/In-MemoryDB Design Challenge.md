###Problem

Link: https://www.1point3acres.com/bbs/thread-1130360-1-1.html

你需要設計並實作一個記憶體中的 key–field–value 資料庫。系統將逐步擴充，從基本 CRUD 功能進化到支援 TTL、時間戳查詢以及備份/還原功能。每個 Level 都要與之前相容。


###Level 1 – Basic CRUD API
####Problem
Requirements
* 支援基本的插入與讀取操作。
* 每個 key 可對應多個 field，但每個 field 只有一個 value。
* 資料存放於記憶體中，應能在常數或接近常數時間內存取。


API:
```java
def set(key: str, field: str, value: str):
    """Store the given value for the specified key and field."""
    pass

def get(key: str, field: str) -> Optional[str]:
    """Retrieve the value for the given key and field, or None if not found."""
    pass

```

Example:  
```java
set("user1", "name", "Alice")
get("user1", "name")  # "Alice"
get("user1", "age")   # None
```

####Solution
```java
import java.util.*;

public class InMemoryDB {
    // 資料結構: key -> field -> value
    private Map<String, Map<String, String>> db;

    public InMemoryDB() {
        db = new HashMap<>();
    }

    public void set(String key, String field, String value) {
        db.computeIfAbsent(key, k -> new HashMap<>()).put(field, value);
    }

    public String get(String key, String field) {
        return db.containsKey(key) ? db.get(key).getOrDefault(field, null) : null;
    }
}
```

Level 1 說明
* 使用 HashMap 實現 O(1) 查找。
* 支援 set() 與 get()，類似 Redis 的 hash 資料結構。

###Level 2 – Scanning Support
####Problem
Requirements
* 回傳指定 key 底下所有的 field–value pair。
* 支援依 field 前綴過濾。

API:
```java
def scan(key: str) -> List[str]:
    """Return all field-value pairs for a key in 'field(value)' format."""
    pass

def scan_with_prefix(key: str, prefix: str) -> List[str]:
    """Return only field-value pairs where the field starts with the given prefix."""
    pass

```

Example:
```java
set("user1", "name", "Alice")
set("user1", "age", "30")
scan("user1")  # ["name(Alice)", "age(30)"]

set("user2", "nickname", "Al")
set("user2", "name", "Alice")
scan_with_prefix("user2", "name")  # ["name(Alice)"]
scan_with_prefix("user2", "x")     # []
```

####Solution
```java
public List<String> scan(String key) {
    if (!db.containsKey(key)) return new ArrayList<>();
    List<String> result = new ArrayList<>();
    for (Map.Entry<String, String> entry : db.get(key).entrySet()) {
        result.add(entry.getKey() + "(" + entry.getValue() + ")");
    }
    return result;
}

public List<String> scanWithPrefix(String key, String prefix) {
    if (!db.containsKey(key)) return new ArrayList<>();
    List<String> result = new ArrayList<>();
    for (Map.Entry<String, String> entry : db.get(key).entrySet()) {
        if (entry.getKey().startsWith(prefix)) {
            result.add(entry.getKey() + "(" + entry.getValue() + ")");
        }
    }
    return result;
}
```

Level 2 說明
* scan() 回傳該 key 下所有的 field(value)。
* scanWithPrefix() 支援 field 的 prefix 過濾。

###Level 3 – Timestamp & TTL Support
####Problem
Requirements
* 支援帶 timestamp 的插入操作。
* 支援 TTL（Time‑To‑Live）：資料會在 timestamp + ttl 時過期，當「當前時間 == timestamp + ttl」即視為過期。
* 時間由傳入的 timestamp 模擬，且保證不倒退。

API:
```java
def set_at(key: str, field: str, value: str, timestamp: int):
    """Insert a record with the specified timestamp."""
    pass

def set_with_ttl(key: str, field: str, value: str, timestamp: int, ttl: int):
    """Insert a record that expires at timestamp + ttl."""
    pass

def get_at(key: str, field: str, timestamp: int) -> Optional[str]:
    """Return the value only if it hasn't expired. Return None if not found or expired."""
    pass

def scan_at(key: str, timestamp: int) -> List[str]:
    """Return all non-expired field-value pairs for a key at a given timestamp."""
    pass

def scan_with_prefix_at(key: str, prefix: str, timestamp: int) -> List[str]:
    """Like scan_with_prefix but only for non-expired records."""
    pass
```

Example:
```java
set_at("user3", "status", "active", 100)
get_at("user3", "status", 101)  # "active"
get_at("user3", "status", 99)   # None

set_with_ttl("user4", "code", "XYZ", 100, 5)
get_at("user4", "code", 104)    # "XYZ"
get_at("user4", "code", 105)    # None

scan_at("user3", 101)  # ["status(active)"]
scan_at("user3", 99)   # []
```

####Solution
```java
class Record {
    String value;
    int timestamp; // 設定時間
    Integer ttl;   // null 表示無 TTL

    public Record(String value, int timestamp, Integer ttl) {
        this.value = value;
        this.timestamp = timestamp;
        this.ttl = ttl;
    }

    public boolean isExpired(int now) {
        return ttl != null && now >= timestamp + ttl;
    }
}
```
更新資料結構並擴充函式
```java
private Map<String, Map<String, Record>> timedDb = new HashMap<>();

public void setAt(String key, String field, String value, int timestamp) {
    timedDb.computeIfAbsent(key, k -> new HashMap<>())
           .put(field, new Record(value, timestamp, null));
}

public void setWithTtl(String key, String field, String value, int timestamp, int ttl) {
    timedDb.computeIfAbsent(key, k -> new HashMap<>())
           .put(field, new Record(value, timestamp, ttl));
}

public String getAt(String key, String field, int timestamp) {
    if (!timedDb.containsKey(key)) return null;
    Record rec = timedDb.get(key).get(field);
    if (rec == null || rec.timestamp > timestamp || rec.isExpired(timestamp)) return null;
    return rec.value;
}

public List<String> scanAt(String key, int timestamp) {
    List<String> result = new ArrayList<>();
    if (!timedDb.containsKey(key)) return result;
    for (Map.Entry<String, Record> entry : timedDb.get(key).entrySet()) {
        Record rec = entry.getValue();
        if (!rec.isExpired(timestamp) && rec.timestamp <= timestamp) {
            result.add(entry.getKey() + "(" + rec.value + ")");
        }
    }
    return result;
}

public List<String> scanWithPrefixAt(String key, String prefix, int timestamp) {
    List<String> result = new ArrayList<>();
    if (!timedDb.containsKey(key)) return result;
    for (Map.Entry<String, Record> entry : timedDb.get(key).entrySet()) {
        Record rec = entry.getValue();
        if (entry.getKey().startsWith(prefix) && !rec.isExpired(timestamp) && rec.timestamp <= timestamp) {
            result.add(entry.getKey() + "(" + rec.value + ")");
        }
    }
    return result;
}
```

Level 3 說明
* Record 包含 timestamp 和 ttl，支援過期邏輯。
* 所有查詢都用傳入的 timestamp 模擬系統時間。

###Level 4 – Backup and Restore
####Problem
Requirements
* 支援在指定 timestamp 時備份整個資料庫狀態。
* 支援還原到最接近且 ≤ timestamp 的最近一次備份。


API:
```java
def backup(timestamp: int)
    """Create a full snapshot of the database at the given timestamp.
    pass

def restore(timestamp: int):
    """Restore the database to the latest backup at or before the given timestamp."""
    pass
```

Example:
```java
set("app", "version", "1.0")
backup(300)
set("app", "version", "2.0")
restore(300)
get("app", "version")  # "1.0"
```

####Solution
```java
private TreeMap<Integer, Map<String, Map<String, Record>>> backups = new TreeMap<>();

public void backup(int timestamp) {
    // 深拷貝整個資料庫
    Map<String, Map<String, Record>> snapshot = new HashMap<>();
    for (String key : timedDb.keySet()) {
        Map<String, Record> inner = new HashMap<>();
        for (Map.Entry<String, Record> entry : timedDb.get(key).entrySet()) {
            Record r = entry.getValue();
            inner.put(entry.getKey(), new Record(r.value, r.timestamp, r.ttl));
        }
        snapshot.put(key, inner);
    }
    backups.put(timestamp, snapshot);
}

public void restore(int timestamp) {
    Integer floor = backups.floorKey(timestamp);
    if (floor == null) return;
    Map<String, Map<String, Record>> backup = backups.get(floor);
    
    // 同樣要深拷貝以避免未來修改
    Map<String, Map<String, Record>> copy = new HashMap<>();
    for (String key : backup.keySet()) {
        Map<String, Record> inner = new HashMap<>();
        for (Map.Entry<String, Record> entry : backup.get(key).entrySet()) {
            Record r = entry.getValue();
            inner.put(entry.getKey(), new Record(r.value, r.timestamp, r.ttl));
        }
        copy.put(key, inner);
    }
    timedDb = copy;
}

```

Level 4 說明
* 使用 TreeMap 來找小於等於的時間點做備份還原。
* 用深拷貝避免之後的修改影響備份。

###Test cases
```java
public static void main(String[] args) {
    InMemoryDB db = new InMemoryDB();

    // Level 1
    db.set("user1", "name", "Alice");
    System.out.println(db.get("user1", "name")); // Alice

    // Level 2
    db.set("user1", "age", "30");
    System.out.println(db.scan("user1")); // [name(Alice), age(30)]
    System.out.println(db.scanWithPrefix("user1", "na")); // [name(Alice)]

    // Level 3
    db.setAt("user2", "status", "active", 100);
    System.out.println(db.getAt("user2", "status", 101)); // active

    db.setWithTtl("user3", "token", "XYZ", 100, 5);
    System.out.println(db.getAt("user3", "token", 104)); // XYZ
    System.out.println(db.getAt("user3", "token", 105)); // null

    // Level 4
    db.set("app", "version", "1.0");
    db.backup(300);
    db.set("app", "version", "2.0");
    db.restore(300);
    System.out.println(db.get("app", "version")); // 1.0
}
```







