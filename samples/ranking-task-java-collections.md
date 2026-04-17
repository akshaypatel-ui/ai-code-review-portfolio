# Ranking Task: Java Collections & ConcurrentModificationException

## 📝 The Prompt
> "An order processing system removes cancelled orders from a list. The code compiles fine but crashes at runtime with a `ConcurrentModificationException`. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public static List<String> removeCancelledOrders(List<String> orders) {
    for (String order : orders) { // Bug here
        if (order.startsWith("CANCELLED")) {
            orders.remove(order); // Triggers ConcurrentModificationException
        }
    }
    return orders;
}
```

---

## 🤖 Model Responses

### Response A
> This is a **ConcurrentModificationException**. The enhanced for-loop uses an `Iterator` behind the scenes. When you call `orders.remove(order)` directly on the list, it increments the list's `modCount` but the iterator is not updated, triggering Java's **fail-fast** mechanism.
> 
> **Fix — Use `removeIf()` (Java 8+):**
> ```java
> orders.removeIf(order -> order.startsWith("CANCELLED"));
> ```
> Alternatively, use an explicit `Iterator` and call `it.remove()`.

### Response B
> You can't modify a list while iterating. Use a regular for-loop with an index:
> ```java
> for (int i = 0; i < orders.size(); i++) {
>     if (orders.get(i).startsWith("CANCELLED")) {
>         orders.remove(i);
>     }
> }
> ```

### Response C
> The for-each loop creates an iterator. The safest modern fix is `removeIf()`. You can also use `Iterator.remove()` which keeps the iterator in sync.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response C > Response B**

### 🧠 Justification

1.  **Explanation Depth**: **Response A** provides the most accurate technical explanation, citing the `modCount` state tracking and the internal use of `Iterator` within enhanced for-loops.
2.  **Detection of Logic Errors**: **Response B is dangerous.** Suggesting a standard `for(int i=0...)` loop for removals introduces the **"Index Skip" bug**. When an element is removed, the tail of the list shifts left, meaning `orders.get(i+1)` becomes the new `orders.get(i)`. The loop's `i++` will then skip checking that shifted element entirely.
3.  **Modern Idioms**: **Response A** and **Response C** correctly prioritize `removeIf()`, which is the thread-safe and idiomatic choice in modern Java.

---
### 🛠 Gold Standard Correction
```java
public static List<String> removeCancelledOrders(List<String> orders) {
    // Standard Modern C#: Thread-safe and handles structural modifications
    orders.removeIf(order -> order.startsWith("CANCELLED"));
    return orders;
}
```

---
*Back to [Samples](./)*
