# Ranking Task: Java List Implementations (Fixed-Size vs. Mutable)

## 📝 The Prompt
> "A content management system initializes a list of tags using `Arrays.asList()` and then tries to add user-specified tags. It compiles fine but throws an `UnsupportedOperationException` at runtime. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public static List<String> initializeTags(String... extraTags) {
    List<String> tags = Arrays.asList("general", "public", "active"); // Bug here
    for (String tag : extraTags) {
        tags.add(tag); // Throws UnsupportedOperationException
    }
    return tags;
}
```

---

## 🤖 Model Responses

### Response A
> This is a **fixed-size list** trap. `Arrays.asList()` returns a wrapper around the original array (`Arrays$ArrayList`), which allows element replacement via `set()` but forbids structural changes like `add()`.
> 
> **Fix — Wrap in a real ArrayList:**
> ```java
> List<String> tags = new ArrayList<>(Arrays.asList("general", "public", "active"));
> ```
> This copies elements into a fully mutable `java.util.ArrayList`.

### Response B
> `Arrays.asList()` returns an immutable list. You need to create a mutable copy.

### Response C
> The issue is that `Arrays.asList()` creates a read-only list. Instead, create the list manually and use `tags.add()` for every element to ensure full control.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response C > Response B**

### 🧠 Justification

1.  **Technical Precision**: **Response A** is the only one to correctly distinguish between a **fixed-size** list and a **truly immutable** list (`List.of`). Knowing that `Arrays.asList` still allows `set()` operations is a key distinction.
2.  **Implementation Knowledge**: Response A identifies the internal class `Arrays$ArrayList`, demonstrating a deep understanding of JDK internals.
3.  **Educational Contrast**: Response A provides a high-value comparison of different list factories across Java versions, which is extremely helpful for developers migrating from legacy to modern Java.

---
### 🛠 Gold Standard Correction
```java
public static List<String> initializeTags(String... extraTags) {
    // Correct: Wrap the fixed-size array view in a mutable ArrayList
    List<String> tags = new ArrayList<>(Arrays.asList("general", "public", "active"));
    
    for (String tag : extraTags) {
        tags.add(tag);
    }
    return tags;
}
```

---
*Back to [Samples](./)*
