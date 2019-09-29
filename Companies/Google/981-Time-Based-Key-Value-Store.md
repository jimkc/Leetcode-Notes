## Question
Create a timebased key-value store class TimeMap, that supports two operations.<br>
<br>
1. set(string key, string value, int timestamp)<br>
<br>
Stores the key and value, along with the given timestamp.<br>
2. get(string key, int timestamp)<br>
<br>
Returns a value such that set(key, value, timestamp_prev) was called previously, with timestamp_prev <= timestamp.<br>
If there are multiple such values, it returns the one with the largest timestamp_prev.<br>
If there are no values, it returns the empty string ("").<br>

**Example :**   
<pre>
Input: inputs = ["TimeMap","set","get","get","set","get","get"], inputs = [[],["foo","bar",1],["foo",1],["foo",3],["foo","bar2",4],["foo",4],["foo",5]]
Output: [null,null,"bar","bar",null,"bar2","bar2"]
Explanation:   
TimeMap kv;   
kv.set("foo", "bar", 1); // store the key "foo" and value "bar" along with timestamp = 1   
kv.get("foo", 1);  // output "bar"   
kv.get("foo", 3); // output "bar" since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 ie "bar"   
kv.set("foo", "bar2", 4);   
kv.get("foo", 4); // output "bar2"   
kv.get("foo", 5); //output "bar2"   

Input: inputs = ["TimeMap","set","set","get","get","get","get","get"], inputs = [[],["love","high",10],["love","low",20],["love",5],["love",10],["love",15],["love",20],["love",25]]
Output: [null,null,null,"","high","high","low","low"]
</pre>

Note:<br>
<br>
All key/value strings are lowercase.<br>
All key/value strings have length in the range [1, 100]<br>
The timestamps for all TimeMap.set operations are strictly increasing.<br>
1 <= timestamp <= 10^7<br>
TimeMap.set and TimeMap.get functions will be called a total of 120000 times (combined) per test case.

## Thinking
**HashMap + Binary Search**
Intuition and Algorithm<br>
<br>
For each key we get or set, we only care about the timestamps and values for that key. We can store this information in a HashMap.<br>
<br>
Now, for each key, we can binary search the sorted list of timestamps to find the relevant value for that key.

## Coding
Time: O(1);O(logn);  </br>
Space: O(n)
```python
class TimeMap:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.seen = collections.defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        self.seen[key].append((timestamp, value))
        

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.seen:
            return ""
        else:
            values = self.seen[key]
            l, r = 0, len(values)-1
            while l<=r:
                mid = r+(l-r)//2
                tup = values[mid]
                time = tup[0]
                val = tup[1]
                if time == timestamp:
                    return val
                elif time > timestamp:
                    r = mid-1
                else:
                    l = mid+1
            
            # right will be the value that is smaller but closest to the timestamp
            if r >= 0:
                return values[r][1]
            else:
                return ""
            
                


# Your TimeMap object will be instantiated and called as such:
# obj = TimeMap()
# obj.set(key,value,timestamp)
# param_2 = obj.get(key,timestamp)
```

