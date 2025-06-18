# ☁️ In-Memory Cloud Storage System – Java Solution

This solution implements a multi-level in-memory cloud storage system that supports file management, file statistics, multi-user management with storage limits, and backup/restore functionality.

---

### ✅ Level 1: Basic File Operations

```java
import java.util.*;

class CloudStorage {
    static class File {
        String name;
        int size;

        File(String name, int size) {
            this.name = name;
            this.size = size;
        }
    }

    Map<String, File> fileMap;

    public CloudStorage() {
        this.fileMap = new HashMap<>();
    }

    // Add a file
    public boolean addFile(String name, int size) {
        if (fileMap.containsKey(name)) return false;
        fileMap.put(name, new File(name, size));
        return true;
    }

    // Get file size
    public Integer getFileSize(String name) {
        File f = fileMap.get(name);
        return (f != null) ? f.size : null;
    }

    // Delete file
    public Integer deleteFile(String name) {
        File f = fileMap.remove(name);
        return (f != null) ? f.size : null;
    }

    // Level 2: Get top N largest files with prefix
    public List<String> getNLargest(String prefix, int n) {
        PriorityQueue<File> pq = new PriorityQueue<>((a, b) -> {
            if (a.size != b.size) return Integer.compare(a.size, b.size);
            return b.name.compareTo(a.name); // reverse lex order
        });

        for (File file : fileMap.values()) {
            if (file.name.startsWith(prefix)) {
                pq.offer(file);
                if (pq.size() > n) pq.poll();
            }
        }

        List<File> result = new ArrayList<>();
        while (!pq.isEmpty()) result.add(pq.poll());
        Collections.reverse(result); // largest first

        List<String> names = new ArrayList<>();
        for (File file : result) {
            names.add(file.name);
        }
        return names;
    }
}
```
### ✅ Level 3: Users with Capacity Limits and Merging

```java
class CloudStorageWithUsers {
    static class File {
        String name;
        int size;

        File(String name, int size) {
            this.name = name;
            this.size = size;
        }
    }

    static class User {
        String name;
        int capacity;
        int usedSpace;
        Map<String, File> files;

        User(String name, int capacity) {
            this.name = name;
            this.capacity = capacity;
            this.usedSpace = 0;
            this.files = new HashMap<>();
        }

        boolean canAddFile(int size) {
            return usedSpace + size <= capacity;
        }

        boolean addFile(String filename, File file) {
            if (files.containsKey(filename) || !canAddFile(file.size)) {
                return false;
            }
            files.put(filename, file);
            usedSpace += file.size;
            return true;
        }

        boolean deleteFile(String filename) {
            File removed = files.remove(filename);
            if (removed != null) {
                usedSpace -= removed.size;
                return true;
            }
            return false;
        }
    }

    Map<String, User> users;

    public CloudStorageWithUsers() {
        this.users = new HashMap<>();
    }

    public boolean addUser(String username, int capacity) {
        if (users.containsKey(username)) return false;
        users.put(username, new User(username, capacity));
        return true;
    }

    public boolean addFile(String username, String filename, int size) {
        User user = users.get(username);
        if (user == null) return false;
        return user.addFile(filename, new File(filename, size));
    }

    public boolean mergeUsers(String u1, String u2, String newUser) {
        if (users.containsKey(newUser)) return false;
        User user1 = users.get(u1), user2 = users.get(u2);
        if (user1 == null || user2 == null) return false;

        int newCapacity = user1.capacity + user2.capacity;
        User merged = new User(newUser, newCapacity);

        for (File f : user1.files.values()) {
            if (!merged.addFile(f.name, new File(f.name, f.size))) return false;
        }
        for (File f : user2.files.values()) {
            if (!merged.addFile(f.name, new File(f.name, f.size))) return false;
        }

        users.put(newUser, merged);
        users.remove(u1);
        users.remove(u2);
        return true;
    }
}
```

###✅ Level 4: Backup and Restore
```java
class CloudStorageWithBackup extends CloudStorageWithUsers {
    static class Backup {
        Map<String, File> backupFiles;

        Backup(Map<String, File> currentFiles) {
            this.backupFiles = new HashMap<>();
            for (Map.Entry<String, File> entry : currentFiles.entrySet()) {
                File f = entry.getValue();
                this.backupFiles.put(entry.getKey(), new File(f.name, f.size));
            }
        }
    }

    Map<String, Backup> backups = new HashMap<>();

    public boolean backup(String username) {
        User user = users.get(username);
        if (user == null) return false;
        backups.put(username, new Backup(user.files));
        return true;
    }

    public boolean restore(String username) {
        User user = users.get(username);
        Backup bkp = backups.get(username);
        if (user == null || bkp == null) return false;

        int totalSize = 0;
        for (File f : bkp.backupFiles.values()) totalSize += f.size;
        if (totalSize > user.capacity) return false;

        user.files.clear();
        user.usedSpace = 0;

        for (File f : bkp.backupFiles.values()) {
            user.files.put(f.name, new File(f.name, f.size));
            user.usedSpace += f.size;
        }
        return true;
    }
}
```