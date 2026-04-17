# Ranking Task: Java Autoboxing & NullPointerException

## 📝 The Prompt
> "A survey analytics module throws an unexpected `NullPointerException` on a line with no method calls: `if (rating >= 1 && rating <= 5)`. Here, `rating` is an `Integer` from a `List<Integer>`. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public static double computeAverage(List<Integer> ratings) {
    for (Integer rating : ratings) {
        // Bug: Implicit unboxing of null Integer
        if (rating >= 1 && rating <= 5) { 
            sum += rating;
        }
    }
}
```

---

## 🤖 Model Responses

### Response A
> This is a **phantom NullPointerException** caused by implicit **unboxing**. The compiler inserts an invisible `rating.intValue()` call to perform the numeric comparison. When `rating` is `null`, this triggers the NPE.
> 
> **Fix — Guard with reference check:**
> ```java
> if (rating != null && rating >= 1 && rating <= 5)
> ```
> The short-circuit `&&` ensures the unboxing operation (`rating.intValue()`) only happens if the reference is not null.

### Response B
> The list contains nulls. Add `if (rating != null)` to skip the null records and save the application from crashing.

### Response C
> Make the unboxing explicit so you can see the error:
> ```java
> if (rating == null) continue;
> int val = rating.intValue();
> ```

---

## ⚖️ My Evaluation

**Ranking: Response A > Response C > Response B**

### 🧠 Justification

1.  **De-mystifying the Stack Trace**: **Response A** is the only one that addresses the developer's core confusion: why a line with no method calls can throw an NPE. By identifying the **compiler-inserted `intValue()`**, it provides the most satisfying technical answer.
2.  **Defensive Programming**: Response A highlights the importance of the short-circuiting `&&` operator as a guard for unmanaged wrapper types.
3.  **Educational Scope**: While **Response C** provides a valid coding pattern by making unboxing explicit, Response A helps the developer understand the *mechanism* of the language, which prevents future bugs across all boxed types (`Long`, `Boolean`, etc.).

---
### 🛠 Gold Standard Correction
```java
public static double computeAverage(List<Integer> ratings) {
    return ratings.stream()
        .filter(Objects::nonNull) // Safe filter to remove null wrappers
        .filter(r -> r >= 1 && r <= 5) // Now safe to unbox
        .mapToInt(Integer::intValue)
        .average()
        .orElse(0.0);
}
```

---
*Back to [Samples](./)*
