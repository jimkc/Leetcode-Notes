### Link
https://www.1point3acres.com/bbs/thread-1133037-1-1.html

### Solution
#### Interleaving iterator
```java
public class InterleavingIterator<T> implements Iterator<T> {
    private Queue<Iterator<T>> queue;

    public InterleavingIterator(List<Iterator<T>> iterators) {
        queue = new LinkedList<>();
        for (Iterator<T> it : iterators) {
            if (it.hasNext()) queue.offer(it);
        }
    }

    @Override
    public boolean hasNext() {
        return !queue.isEmpty();
    }

    @Override
    public T next() {
        if (!hasNext()) throw new NoSuchElementException();

        Iterator<T> current = queue.poll();
        T val = current.next();
        if (current.hasNext()) {
            queue.offer(current); // 加回去等下一輪
        }
        return val;
    }
}
```

#### Cyclic iterator
```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

/**
 * A CyclicIterator continuously iterates over the elements provided by an input iterator,
 * restarting from the beginning once it reaches the end.
 * It stores all elements from the input iterator into an internal list to enable cycling.
 *
 * @param <T> The type of elements being iterated.
 */
public class CyclicIterator<T> implements Iterator<T> {
    private final List<T> elements; // Stores all elements to enable cycling
    private int currentIndex;       // Current position in the elements list

    /**
     * Constructs a CyclicIterator.
     * It consumes all elements from the input iterator immediately.
     *
     * @param inputIterator The iterator whose elements will be cycled.
     * @throws IllegalArgumentException if the inputIterator is null.
     */
    public CyclicIterator(Iterator<T> inputIterator) {
        if (inputIterator == null) {
            throw new IllegalArgumentException("Input iterator cannot be null.");
        }
        this.elements = new ArrayList<>();
        // Consume all elements from the input iterator
        while (inputIterator.hasNext()) {
            this.elements.add(inputIterator.next());
        }
        this.currentIndex = 0; // Start at the beginning
    }

    @Override
    public boolean hasNext() {
        // A cyclic iterator always has a "next" if it has any elements at all.
        // It will cycle indefinitely.
        return !elements.isEmpty();
    }

    @Override
    public T next() {
        if (!hasNext()) {
            throw new NoSuchElementException("CyclicIterator has no elements to cycle through.");
        }

        T element = elements.get(currentIndex);
        currentIndex = (currentIndex + 1) % elements.size(); // Move to the next element, wrapping around
        return element;
    }

    // Main method for testing CyclicIterator
    public static void main(String[] args) {
        System.out.println("--- Testing CyclicIterator ---");

        // Test Case 1: Standard cycling
        List<String> words = List.of("apple", "banana", "cherry");
        CyclicIterator<String> cyclicWords = new CyclicIterator<>(words.iterator());

        System.out.println("Cycling through words 7 times:");
        for (int i = 0; i < 7; i++) {
            if (cyclicWords.hasNext()) {
                System.out.print(cyclicWords.next() + " ");
            }
        }
        System.out.println("\nExpected: apple banana cherry apple banana cherry apple"); // Note: last 'apple' is the start of the next cycle

        // Test Case 2: Empty input
        List<Integer> emptyList = List.of();
        CyclicIterator<Integer> cyclicEmpty = new CyclicIterator<>(emptyList.iterator());
        System.out.println("\nCycling through empty list:");
        System.out.println("Has next: " + cyclicEmpty.hasNext()); // Expected: false
        try {
            cyclicEmpty.next();
        } catch (NoSuchElementException e) {
            System.out.println("Caught expected exception: " + e.getMessage()); // Expected: exception
        }

        // Test Case 3: Single element
        List<Double> single = List.of(3.14);
        CyclicIterator<Double> cyclicSingle = new CyclicIterator<>(single.iterator());
        System.out.println("\nCycling through single element 4 times:");
        for (int i = 0; i < 4; i++) {
            if (cyclicSingle.hasNext()) {
                System.out.print(cyclicSingle.next() + " ");
            }
        }
        System.out.println("\nExpected: 3.14 3.14 3.14 3.14");
    }
}
```

####  Step Iterator
Concept: A StepIterator takes a single Iterator<T> and a step value. It iterates through the input iterator but only yields elements at intervals defined by the step. For example, if step is 2, it yields the 1st, 3rd, 5th elements, and so on.

Challenge: We need to advance the underlying iterator internally without yielding those skipped elements.

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

/**
 * A StepIterator iterates over the elements of an input iterator,
 * yielding elements at specified intervals (steps).
 * For example, a step of 1 returns every element, a step of 2 returns every other.
 *
 * @param <T> The type of elements being iterated.
 */
public class StepIterator<T> implements Iterator<T> {
    private final Iterator<T> inputIterator;
    private final int step;
    private T nextElement; // Stores the next element to be returned

    /**
     * Constructs a StepIterator.
     *
     * @param inputIterator The iterator to step through.
     * @param step          The number of elements to skip after yielding one. Must be >= 0.
     * A step of 0 means every element.
     * @throws IllegalArgumentException if inputIterator is null or step is negative.
     */
    public StepIterator(Iterator<T> inputIterator, int step) {
        if (inputIterator == null) {
            throw new IllegalArgumentException("Input iterator cannot be null.");
        }
        if (step < 0) {
            throw new IllegalArgumentException("Step must be a non-negative integer.");
        }
        this.inputIterator = inputIterator;
        // The 'step' parameter usually means "take one, then skip 'step' elements".
        // If step is 0, we effectively take every element.
        // If step is 1, we take one, skip one, take one, etc.
        this.step = step;
        findNext(); // Pre-fetch the first element
    }

    /**
     * Helper method to find and store the next element that should be yielded.
     */
    private void findNext() {
        nextElement = null; // Reset
        if (inputIterator.hasNext()) {
            nextElement = inputIterator.next(); // Get the current element

            // If step is greater than 0, skip 'step' more elements
            for (int i = 0; i < step; i++) {
                if (inputIterator.hasNext()) {
                    inputIterator.next(); // Skip this element
                } else {
                    break; // No more elements to skip
                }
            }
        }
    }

    @Override
    public boolean hasNext() {
        return nextElement != null; // True if we have a pre-fetched next element
    }

    @Override
    public T next() {
        if (!hasNext()) {
            throw new NoSuchElementException("No more elements in StepIterator.");
        }
        T current = nextElement;
        findNext(); // Prepare for the next call to next()
        return current;
    }

    // Main method for testing StepIterator
    public static void main(String[] args) {
        System.out.println("--- Testing StepIterator ---");

        // Test Case 1: Step 0 (every element)
        List<Integer> numbers1 = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        StepIterator<Integer> step0 = new StepIterator<>(numbers1.iterator(), 0);
        System.out.print("Step 0: ");
        while (step0.hasNext()) {
            System.out.print(step0.next() + " ");
        }
        System.out.println("\nExpected: 1 2 3 4 5 6 7 8 9 10");

        // Test Case 2: Step 1 (every other element)
        List<String> letters = List.of("A", "B", "C", "D", "E", "F", "G");
        StepIterator<String> step1 = new StepIterator<>(letters.iterator(), 1);
        System.out.print("Step 1: ");
        while (step1.hasNext()) {
            System.out.print(step1.next() + " ");
        }
        System.out.println("\nExpected: A C E G");

        // Test Case 3: Step 2 (take one, skip two)
        List<Integer> numbers2 = Arrays.asList(10, 20, 30, 40, 50, 60, 70, 80);
        StepIterator<Integer> step2 = new StepIterator<>(numbers2.iterator(), 2);
        System.out.print("Step 2: ");
        while (step2.hasNext()) {
            System.out.print(step2.next() + " ");
        }
        System.out.println("\nExpected: 10 40 70");

        // Test Case 4: Step larger than remaining elements
        List<Character> chars = List.of('X', 'Y', 'Z');
        StepIterator<Character> step5 = new StepIterator<>(chars.iterator(), 5);
        System.out.print("Step 5: ");
        while (step5.hasNext()) {
            System.out.print(step5.next() + " ");
        }
        System.out.println("\nExpected: X");

        // Test Case 5: Empty input
        List<String> empty = List.of();
        StepIterator<String> stepEmpty = new StepIterator<>(empty.iterator(), 1);
        System.out.println("\nStep Empty list (step 1): " + stepEmpty.hasNext()); // Expected: false
    }
}
```
### Odd or Even Value Iterator
Concept: This iterator will specifically work with Number types (like Integer, Long, etc.) that are comparable. It takes an input Iterator<Number> and a boolean flag (isOdd). It will then only yield numbers from the input that are either odd or even, depending on the isOdd flag.

Challenge: We need to filter elements on the fly. This will also use the pre-fetching pattern.

```java
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

/**
 * An OddEvenValueIterator filters elements from an input iterator,
 * yielding only odd or only even numbers based on a specified criterion.
 * It assumes the input iterator provides numbers that can be checked for parity.
 *
 * @param <T> The type of elements being iterated, must extend Number and be Comparable.
 */
public class OddEvenValueIterator<T extends Number & Comparable<T>> implements Iterator<T> {
    private final Iterator<T> inputIterator;
    private final boolean filterForOdd; // true for odd, false for even
    private T nextElement; // Stores the next element to be returned

    /**
     * Constructs an OddEvenValueIterator.
     *
     * @param inputIterator The iterator to filter.
     * @param filterForOdd  If true, only odd numbers are yielded. If false, only even numbers.
     * @throws IllegalArgumentException if inputIterator is null.
     */
    public OddEvenValueIterator(Iterator<T> inputIterator, boolean filterForOdd) {
        if (inputIterator == null) {
            throw new IllegalArgumentException("Input iterator cannot be null.");
        }
        this.inputIterator = inputIterator;
        this.filterForOdd = filterForOdd;
        findNext(); // Pre-fetch the first valid element
    }

    /**
     * Helper method to find and store the next element that satisfies the odd/even condition.
     */
    private void findNext() {
        nextElement = null; // Reset
        while (inputIterator.hasNext()) {
            T candidate = inputIterator.next();
            // We use longValue() to safely handle different Number types (Integer, Long, etc.)
            long value = candidate.longValue();

            boolean isCandidateOdd = (value % 2 != 0);

            if (filterForOdd == isCandidateOdd) {
                // Found a matching element
                nextElement = candidate;
                return; // Stop searching
            }
        }
        // If loop finishes, no more matching elements found, nextElement remains null
    }

    @Override
    public boolean hasNext() {
        return nextElement != null; // True if we have a pre-fetched next element
    }

    @Override
    public T next() {
        if (!hasNext()) {
            throw new NoSuchElementException("No more matching elements in OddEvenValueIterator.");
        }
        T current = nextElement;
        findNext(); // Prepare for the next call to next()
        return current;
    }

    // Main method for testing OddEvenValueIterator
    public static void main(String[] args) {
        System.out.println("--- Testing OddEvenValueIterator ---");

        List<Integer> mixedNumbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12);

        // Test Case 1: Filter for Odd numbers
        OddEvenValueIterator<Integer> oddIterator = new OddEvenValueIterator<>(mixedNumbers.iterator(), true);
        System.out.print("Odd numbers: ");
        while (oddIterator.hasNext()) {
            System.out.print(oddIterator.next() + " ");
        }
        System.out.println("\nExpected: 1 3 5 7 9 11");

        // Test Case 2: Filter for Even numbers
        OddEvenValueIterator<Integer> evenIterator = new OddEvenValueIterator<>(mixedNumbers.iterator(), false);
        System.out.print("Even numbers: ");
        while (evenIterator.hasNext()) {
            System.out.print(evenIterator.next() + " ");
        }
        System.out.println("\nExpected: 2 4 6 8 10 12");

        // Test Case 3: Empty input
        List<Long> emptyList = List.of();
        OddEvenValueIterator<Long> emptyOdd = new OddEvenValueIterator<>(emptyList.iterator(), true);
        System.out.println("\nEmpty list (odd): " + emptyOdd.hasNext()); // Expected: false

        // Test Case 4: List with no matching elements
        List<Integer> onlyEvens = List.of(2, 4, 6);
        OddEvenValueIterator<Integer> oddFromEvens = new OddEvenValueIterator<>(onlyEvens.iterator(), true);
        System.out.print("Odd from only evens: ");
        while (oddFromEvens.hasNext()) {
            System.out.print(oddFromEvens.next() + " ");
        }
        System.out.println("\nExpected: (nothing)");

        List<Integer> onlyOdds = List.of(1, 3, 5);
        OddEvenValueIterator<Integer> evenFromOdds = new OddEvenValueIterator<>(onlyOdds.iterator(), false);
        System.out.print("Even from only odds: ");
        while (evenFromOdds.hasNext()) {
            System.out.print(evenFromOdds.next() + " ");
        }
        System.out.println("\nExpected: (nothing)");
    }
}
```



