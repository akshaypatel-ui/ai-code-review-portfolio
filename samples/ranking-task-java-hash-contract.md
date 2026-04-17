# Ranking Task: Java Equals & HashCode Contract

## 📝 The Prompt
> "An HR system uses a `HashSet` to store employees. Despite overriding `equals()`, the set is allowing duplicate entries with the same ID. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public class Employee {
    private int id;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Employee)) return false;
        Employee other = (Employee) obj;
        return this.id == other.id;
    }
    // Missing: hashCode() override
}
```

---

## 🤖 Model Responses

### Response A
> This is an **equals/hashCode contract violation**. If `a.equals(b)` is true, then `a.hashCode()` must equal `b.hashCode()`.
> 
> **The Root Cause**: `HashSet` first uses `hashCode()` to determine which **bucket** to check. If two objects have different hash codes (which they do by default), `equals()` is never even called.
> 
> **Fix — Override `hashCode()` using the same fields:**
> ```java
> @Override
> public int hashCode() {
>     return Objects.hash(id);
> }
> ```

### Response B
> `HashSet` requires `hashCode()`. Add this method to fix the duplicate issue by returning the ID as the hash.

### Response C
> The issue is that `HashSet` doesn't always use `equals()`. Use a `TreeSet` instead, which uses `compareTo()` to find duplicates.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response B > Response C**

### 🧠 Justification

1.  **Technical Root Cause**: **Response A** provides the most valuable insight by explaining the "bucket" mechanism of hash-based collections. This helps the developer understand why `equals()` was never invoked despite being correctly implemented.
2.  **Contractual Integrity**: Response A explicitly cites the formal Java contract between the two methods, which is critical for senior-level engineering.
3.  **Critique of Workarounds**: **Response C** suggests a dangerous workaround (switching to `TreeSet`) that fixes the symptom but leaves the `Employee` object fundamentally broken for all other hash-based utilities in the ecosystem.

---
### 🛠 Gold Standard Correction
```java
import java.util.Objects;

public class Employee {
    private int id;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Employee)) return false;
        Employee other = (Employee) obj;
        return this.id == other.id;
    }

    // Always override together to maintain the hashing contract
    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

---
*Back to [Samples](./)*
