# Ranking Task: Java Exception Handling & Resource Leaks

## 📝 The Prompt
> "A file processing service silently produces empty results in production with no error messages. The developer added a `try-catch` block but the application still fails silently. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public static List<String> readConfig(String filePath) {
    List<String> lines = new ArrayList<>();
    try {
        BufferedReader reader = new BufferedReader(new FileReader(filePath));
        // ... logic ...
        reader.close(); // Bug: Never called on exception
    } catch (Exception e) {
        // Bug: Swallowed exception
    }
    return lines;
}
```

---

## 🤖 Model Responses

### Response A
> This code has **two major bugs**: a **swallowed exception** (the silent killer) and a **resource leak**.
> 
> **The Root Cause**: By catching `Exception` and doing nothing, you are violating the **fail-fast principle**. When the file isn't found, the method returns an empty list instead of notifying the caller of the failure.
> 
> **Fix — Try-with-resources & Error Propagation:**
> ```java
> try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
>     // ...
> } // Automatically closes. IOException propagated to caller.
> ```

### Response B
> The catch block is empty. Use `e.printStackTrace()` so you can see the error in the console.

### Response C
> The issue is the catch block. Remove the `try-catch` entirely and declare `throws Exception` on the method signature.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response C > Response B**

### 🧠 Justification

1.  **Fail-Fast Advocacy**: **Response A** correctly identifies that "silent failure" is an architectural flaw. Masking a critical configuration failure as an "empty list" leads to unstable system behavior.
2.  **Modern Resource Management**: Response A uses **try-with-resources** (Java 7+), ensuring file handles are released even during IO failures. **Response C** fails to address the resource leak, leaving handles open if the read operation fails.
3.  **Specific Exceptions**: Response A appropriately propagates `IOException`, whereas Response C suggests the overly broad `throws Exception`, which is bad practice for production-grade libraries.

---
### 🛠 Gold Standard Correction
```java
import java.io.IOException;

public static List<String> readConfig(String filePath) throws IOException {
    List<String> lines = new ArrayList<>();
    // try-with-resources ensures handle closure
    try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
        String line;
        while ((line = reader.readLine()) != null) {
            lines.add(line.trim());
        }
    }
    return lines;
}
```

---
*Back to [Samples](./)*
