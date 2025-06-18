###Problem

1. Given multiple NFTs with attributes, create all possible combination of new nfts this given attributes
2. Followup: usage limit on some attributes


###Solution 1
Example input
```java
Map<String, List<String>> attributes = new HashMap<>();
attributes.put("Background", List.of("Blue", "Red"));
attributes.put("Eyes", List.of("Big", "Small"));
attributes.put("Mouth", List.of("Smile", "Frown"));

```

```java
import java.util.*;

public class NFTCombinations {

    public static void main(String[] args) {
        Map<String, List<String>> attributes = new LinkedHashMap<>();
        attributes.put("Background", List.of("Blue", "Red"));
        attributes.put("Eyes", List.of("Big", "Small"));
        attributes.put("Mouth", List.of("Smile", "Frown"));

        List<Map<String, String>> combinations = generateCombinations(attributes);
        printCombinations(combinations);
    }

    public static List<Map<String, String>> generateCombinations(Map<String, List<String>> attributes) {
        List<Map<String, String>> result = new ArrayList<>();
        backtrack(new ArrayList<>(attributes.keySet()), attributes, 0, new HashMap<>(), result);
        return result;
    }

    private static void backtrack(List<String> keys, Map<String, List<String>> attributes,
                                  int index, Map<String, String> current,
                                  List<Map<String, String>> result) {
        if (index == keys.size()) {
            result.add(new LinkedHashMap<>(current));
            return;
        }

        String key = keys.get(index);
        for (String value : attributes.get(key)) {
            current.put(key, value);
            backtrack(keys, attributes, index + 1, current, result);
            current.remove(key); // backtrack
        }
    }

    private static void printCombinations(List<Map<String, String>> combinations) {
        int count = 1;
        for (Map<String, String> combo : combinations) {
            System.out.println("NFT #" + count++);
            for (Map.Entry<String, String> entry : combo.entrySet()) {
                System.out.println(" - " + entry.getKey() + ": " + entry.getValue());
            }
            System.out.println();
        }
    }
}

```


###Follow up
1. Each attribute value has a maximum usage limit.
2. We must generate combinations such that no attribute value exceeds its usage limit.
3. The number of combinations may be less than the full Cartesian product due to the limits.

Example
```java
Map<String, List<String>> attributes = new LinkedHashMap<>();
attributes.put("Background", List.of("Blue", "Red"));
attributes.put("Eyes", List.of("Big", "Small"));
attributes.put("Mouth", List.of("Smile", "Frown"));

Map<String, Map<String, Integer>> usageLimits = new HashMap<>();
usageLimits.put("Background", Map.of("Blue", 2, "Red", 1));
usageLimits.put("Eyes", Map.of("Big", 2, "Small", 1));
usageLimits.put("Mouth", Map.of("Smile", 1, "Frown", 2));
```

Solution
```java
import java.util.*;

public class NFTCombinationsWithLimits {

    public static void main(String[] args) {
        Map<String, List<String>> attributes = new LinkedHashMap<>();
        attributes.put("Background", List.of("Blue", "Red"));
        attributes.put("Eyes", List.of("Big", "Small"));
        attributes.put("Mouth", List.of("Smile", "Frown"));

        Map<String, Map<String, Integer>> usageLimits = new HashMap<>();
        usageLimits.put("Background", new HashMap<>(Map.of("Blue", 2, "Red", 1)));
        usageLimits.put("Eyes", new HashMap<>(Map.of("Big", 2, "Small", 1)));
        usageLimits.put("Mouth", new HashMap<>(Map.of("Smile", 1, "Frown", 2)));

        List<Map<String, String>> combinations = generateCombinations(attributes, usageLimits);
        printCombinations(combinations);
    }

    public static List<Map<String, String>> generateCombinations(
            Map<String, List<String>> attributes,
            Map<String, Map<String, Integer>> usageLimits) {

        List<Map<String, String>> result = new ArrayList<>();
        Map<String, String> current = new LinkedHashMap<>();

        // Track current usage
        Map<String, Map<String, Integer>> currentUsage = new HashMap<>();
        for (String key : usageLimits.keySet()) {
            currentUsage.put(key, new HashMap<>());
        }

        backtrack(new ArrayList<>(attributes.keySet()), attributes, usageLimits,
                  currentUsage, 0, current, result);
        return result;
    }

    private static void backtrack(List<String> keys,
                                  Map<String, List<String>> attributes,
                                  Map<String, Map<String, Integer>> usageLimits,
                                  Map<String, Map<String, Integer>> currentUsage,
                                  int index,
                                  Map<String, String> current,
                                  List<Map<String, String>> result) {

        if (index == keys.size()) {
            result.add(new LinkedHashMap<>(current));
            return;
        }

        String key = keys.get(index);
        for (String value : attributes.get(key)) {
            int used = currentUsage.get(key).getOrDefault(value, 0);
            int limit = usageLimits.getOrDefault(key, Map.of()).getOrDefault(value, Integer.MAX_VALUE);

            if (used >= limit) continue;

            // Use this value
            current.put(key, value);
            currentUsage.get(key).put(value, used + 1);

            backtrack(keys, attributes, usageLimits, currentUsage, index + 1, current, result);

            // Backtrack
            current.remove(key);
            currentUsage.get(key).put(value, used);
        }
    }

    private static void printCombinations(List<Map<String, String>> combinations) {
        int count = 1;
        for (Map<String, String> combo : combinations) {
            System.out.println("NFT #" + count++);
            for (Map.Entry<String, String> entry : combo.entrySet()) {
                System.out.println(" - " + entry.getKey() + ": " + entry.getValue());
            }
            System.out.println();
        }
    }
}
```






