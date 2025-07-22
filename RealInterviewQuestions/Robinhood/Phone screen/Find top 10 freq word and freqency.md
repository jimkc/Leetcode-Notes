### Link
https://www.1point3acres.com/bbs/thread-1128257-1-1.html

### Question
给定一个input str是一本书的一个段落，要求处理input str，extract word，返回top 10 freq word 和 frequency，这个word要lowercase。

思路就是先处理input str，忽略标点符号和空格拿到word，然后把word处理成lowercase，之后用map来存每个word的frequency，然后heapify，再取top 10 words。

Follow up 1：如果input str too big to store in memory怎么办，within one machine，maintain 一个global count map，然后batch处理input str，update map之后算top 10 words

Follow up 2：如果一台机器也处理不过来怎么办，我说用redis来存global count map，每台机器处理完update redis。又问怎么handle failure，机器挂了怎么办，答定期给机器snapshot保存进度，挂了再根据snapshot restore progress

### Solution for base question
```java
import java.util.Comparator;
import java.util.HashMap;
import java.util.Map;
import java.util.PriorityQueue;
import java.util.List;
import java.util.ArrayList;

public class TopFrequentWords {

    // A simple class to hold word and its frequency for easier sorting
    static class WordFrequency {
        String word;
        int frequency;

        public WordFrequency(String word, int frequency) {
            this.word = word;
            this.frequency = frequency;
        }

        @Override
        public String toString() {
            return "'" + word + "': " + frequency;
        }
    }

    public static List<WordFrequency> getTop10FrequentWords(String paragraph) {
        // 1. Normalize the input string: lowercase and remove punctuation
        // Replace all non-alphanumeric characters (except underscore) with a space
        String cleanedParagraph = paragraph.toLowerCase().replaceAll("[^a-z0-9_]+", " ");

        // Replace multiple spaces with a single space and trim leading/trailing spaces
        cleanedParagraph = cleanedParagraph.replaceAll("\\s+", " ").trim();

        // If the paragraph becomes empty after cleaning, return an empty list
        if (cleanedParagraph.isEmpty()) {
            return new ArrayList<>();
        }

        // 2. Extract words
        String[] words = cleanedParagraph.split(" ");

        // 3. Count frequencies using a HashMap
        Map<String, Integer> wordFrequencies = new HashMap<>();
        for (String word : words) {
            // Ensure the word is not empty (can happen if there were leading/trailing spaces or multiple delimiters)
            if (!word.isEmpty()) {
                wordFrequencies.put(word, wordFrequencies.getOrDefault(word, 0) + 1);
            }
        }

        // 4. Find top 10 using a Min-Heap (PriorityQueue)
        // The heap will store WordFrequency objects.
        // We want a min-heap so that the smallest element (least frequent) is at the top.
        // If we exceed 10 elements, we remove the smallest.
        PriorityQueue<WordFrequency> minHeap = new PriorityQueue<>(
            Comparator.comparingInt(wf -> wf.frequency) // Compare by frequency
        );

        for (Map.Entry<String, Integer> entry : wordFrequencies.entrySet()) {
            WordFrequency wf = new WordFrequency(entry.getKey(), entry.getValue());

            minHeap.offer(wf); // Add the current word's frequency

            // If the heap size exceeds 10, remove the element with the smallest frequency
            if (minHeap.size() > 10) {
                minHeap.poll();
            }
        }

        // Convert the heap to a list and reverse it to get top 10 in descending order of frequency
        List<WordFrequency> topWords = new ArrayList<>(minHeap);
        topWords.sort(Comparator.comparingInt(wf -> wf.frequency).reversed()); // Sort in descending order

        return topWords;
    }

    public static void main(String[] args) {
        String paragraph = "This is a test paragraph, which contains many words. " +
                           "This paragraph is just a test, a test for frequency counting. " +
                           "Words like 'test' and 'a' and 'this' will appear frequently. " +
                           "Let's see how many times 'is' and 'paragraph' appear. " +
                           "It's an interesting paragraph for sure! Test, Test, Test.";

        List<WordFrequency> top10 = getTop10FrequentWords(paragraph);
        System.out.println("Top 10 most frequent words:");
        for (WordFrequency wf : top10) {
            System.out.println(wf);
        }

        System.out.println("\n--- Another Example ---");
        String paragraph2 = "apple banana orange apple grape banana apple orange apple banana";
        List<WordFrequency> top10_2 = getTop10FrequentWords(paragraph2);
        System.out.println("Top 10 most frequent words:");
        for (WordFrequency wf : top10_2) {
            System.out.println(wf);
        }

        System.out.println("\n--- Empty Paragraph Example ---");
        String paragraph3 = "!@#$%^&*()"; // Only punctuation
        List<WordFrequency> top10_3 = getTop10FrequentWords(paragraph3);
        System.out.println("Top 10 most frequent words:");
        if (top10_3.isEmpty()) {
            System.out.println("No words found.");
        } else {
            for (WordFrequency wf : top10_3) {
                System.out.println(wf);
            }
        }
    }
}
```