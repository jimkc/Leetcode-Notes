## Question
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.<br>

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.<br>
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.<br>

Follow up:<br>
Could you do both operations in O(1) time complexity?<br>

**Example :**   
<pre>
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
</pre>

## Thinking


## Coding
Time: O(1);
Space: O(1)
```python3
class Node:
    def __init__(self, k, v):
        self.key = k
        self.value = v
        self.prev = None
        self.next = None
        
class LRUCache:
    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.capacity = capacity
        self.dic = {}
        self.head = Node(0,0)
        self.tail = Node(0,0)
        self.head.next = self.tail
        self.tail.prev = self.head
        

    def get(self, key):
        """
        :type key: int
        :rtype: int
        """
        if key in self.dic:
            n = self.dic[key]
            self.remove_node(n)
            self.add_last(n)
            return n.value
        else:
            return -1
        
    def put(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: void
        """
        if key in self.dic:
            self.remove_node(self.dic[key])
        n = Node(key,value)
        self.dic[key] = n
        self.add_last(n)

        if len(self.dic) > self.capacity:
            n = self.head.next
            self.remove_node(n)
            del self.dic[n.key]
            
        
        
        
    def remove_node(self, node):
        pre = node.prev
        nxt = node.next
        pre.next = nxt
        nxt.prev = pre
    
    def add_last(self, node):
        pre = self.tail.prev
        pre.next = node
        node.prev = pre
        node.next = self.tail
        self.tail.prev = node


# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

Method2 :
```python3
def __init__(self, capacity):
    self.dic = collections.OrderedDict()
    self.remain = capacity

def get(self, key):
    if key not in self.dic:
        return -1
    v = self.dic.pop(key) 
    self.dic[key] = v   # set key as the newest one
    return v

def set(self, key, value):
    if key in self.dic:    
        self.dic.pop(key)
    else:
        if self.remain > 0:
            self.remain -= 1  
        else:  # self.dic is full
            self.dic.popitem(last=False) 
    self.dic[key] = value


```
