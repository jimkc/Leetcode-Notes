###Link
https://www.1point3acres.com/bbs/thread-1133037-1-1.html

###Solution
####Interleaving iterator
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