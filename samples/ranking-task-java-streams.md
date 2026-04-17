# Ranking Task: Java Streams (Single-Use Lifecycle)

## 📝 The Prompt
> "A reporting module uses a Stream to filter sales transactions. It first calls `.count()`, then attempts to calculate a sum using the same stream object. It crashes with an `IllegalStateException`. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
Stream<String> transactionStream = transactions.stream()
    .filter(t -> t.startsWith("SALE"));

long count = transactionStream.count(); // Terminal operation 1
double total = transactionStream.sum(); // Bug: Stream already closed
```

---

## 🤖 Model Responses

### Response A
> This is a **stream reuse error**. Java streams can only be consumed once. Terminal operations (like `.count()`) consume and **close** the stream.
> 
> **The Fix — Single Pass Collection:**
> Collect the filtered results into a `List` first. This avoids re-running the `.filter()` logic and allows multiple passes over the data.
> ```java
> List<String> sales = transactions.stream()
>     .filter(t -> t.startsWith("SALE"))
>     .collect(Collectors.toList());
> ```

### Response B
> You can't use a stream twice. Just call `.stream()` again for the second operation.

### Response C
> The stream is closed after the first terminal operation. Use a `Supplier<Stream>` factory to recreate the stream whenever you need to query it.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response C > Response B**

### 🧠 Justification

1.  **Efficiency Awareness**: **Response A** correctly identifies the performance trade-off. Simply recreating the stream (Response B) forces the application to re-run the `.filter()` operation, doubling the CPU cost. Materializing to a list is the "Senior Engineer" approach for multi-pass data.
2.  **Lifecycle Explanation**: Response A provides the best technical depth regarding the transition from lazy intermediate operations to consuming terminal operations.
3.  **Conceptual Precision**: While **Response C** provides a technically valid `Supplier` pattern, it masks the underlying efficiency problem. Response A teaches the developer about **Data vs. Computation Flow**.

---
### 🛠 Gold Standard Correction
```java
public static void generateReport(List<String> transactions) {
    // 1. Filter and Materialize (Filter runs once)
    List<String> sales = transactions.stream()
        .filter(t -> t.startsWith("SALE"))
        .collect(Collectors.toList());

    // 2. Perform multiple operations on the materialized list
    long count = sales.size();
    double total = sales.stream()
        .mapToDouble(s -> parse(s))
        .sum();
}
```

---
*Back to [Samples](./)*
