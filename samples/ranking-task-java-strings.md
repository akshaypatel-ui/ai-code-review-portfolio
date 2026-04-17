# Ranking Task: Java String Semantics (== vs .equals)

## 📝 The Prompt
> "A login system validates user credentials. It works for some users but fails for others, even when they type the correct password. The developer is baffled because the password clearly matches. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public boolean authenticate(String username, String password) {
    String storedPassword = fetchPasswordFromDatabase(username);
    if (password == storedPassword) { // Bug: Reference comparison
        return true;
    }
    return false;
}
```

---

## 🤖 Model Responses

### Response A
> This is the classic **`==` vs `.equals()`** bug in Java. The `==` operator compares **object references** (memory addresses), not string content.
> 
> The password literal `"secret123"` lives in the **String pool**, while `fetchPasswordFromDatabase` returns a new object on the heap. They contain the same characters but are different objects.
> 
> **Fix — use `.equals()` or `Objects.equals()`:**
> ```java
> if (Objects.equals(password, storedPassword)) { 
>     return true; 
> }
> ```

### Response B
> The `==` operator doesn't compare the text of the strings. Change it to `.equals()`.

### Response C
> The problem is that `fetchPasswordFromDatabase` uses `new String()`. You should use string literals directly. With literals, `==` works correctly because Java interns them.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response B > Response C**

### 🧠 Justification

1.  **Technical Root Cause**: **Response A** provides a masterful explanation of the **String Pool** and **Interning**, explaining exactly why this bug is intermittent (it works for hardcoded literals but fails for real data).
2.  **Safety & Robustness**: Response A suggests `Objects.equals()`, which is a modern Java best practice as it prevents `NullPointerException` risks.
3.  **Correcting Misconceptions**: **Response C** provides dangerous advice by suggesting the fix is to use literals so that `==` works. This teaches a brittle, non-production-ready pattern that fails as soon as strings are pulled from a database or user input.

---
### 🛠 Gold Standard Correction
```java
import java.util.Objects;

public boolean authenticate(String username, String password) {
    String storedPassword = fetchPasswordFromDatabase(username);
    
    // Use Objects.equals for null-safe value comparison
    return Objects.equals(password, storedPassword);
}
```

---
*Back to [Samples](./)*
