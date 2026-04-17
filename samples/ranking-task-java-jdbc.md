# Ranking Task: Java JDBC & Resource Leaks

## 📝 The Prompt
> "A database utility using `DriverManager` runs out of connections under load, throwing 'Too many open connections.' Despite calling `.close()` at the end of the method, the leak persists. Identify the bug and provide a corrected version."

### 🚩 Original Code Snippet:
```java
public List<String> findActiveUsers() throws SQLException {
    Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
    PreparedStatement stmt = conn.prepareStatement("...");
    ResultSet rs = stmt.executeQuery();
    // ... logic ...
    rs.close(); 
    stmt.close(); 
    conn.close(); // Bug: Only reached if no exceptions occur
}
```

---

## 🤖 Model Responses

### Response A
> This is a **resource leak** caused by the "happy-path-only" cleanup. If any exception occurs during processing, the `close()` calls are skipped.
> 
> **Fix — Try-with-resources (Java 7+):**
> ```java
> try (Connection conn = DriverManager.getConnection(...);
>      PreparedStatement stmt = conn.prepareStatement(...)) {
>     try (ResultSet rs = stmt.executeQuery()) {
>         // ...
>     }
> }
> ```
> This guarantees closure in reverse order and handles suppressed exceptions.

### Response B
> Use a `finally` block to wrap your close calls. This ensures they run even if an exception is thrown in the middle of the method.

### Response C
> The issue is opening 500 connections. Cache the connection in a static variable so you only ever have one connection open at a time for your entire repository.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response B > Response C**

### 🧠 Justification

1.  **Reliability & Mechanics**: **Response A** is the only one that explains the mechanics of **Try-with-resources**, specifically the reverse-order closing and suppressed exception handling.
2.  **Concurrency Awareness**: **Response C** suggests a dangerous anti-pattern. Sharing a single `Connection` as a static/shared variable is not thread-safe and will cause failures in concurrent environments (race conditions, state corruption).
3.  **Modern Standards**: Response A correctly identifies that manual `finally` blocks (Response B) are outdated and more error-prone than the newer, language-level resource management features.

---
### 🛠 Gold Standard Correction
```java
// Production Best Practice: Use a Connection Pool (HikariCP) instead of DriverManager
public List<String> findActiveUsers() throws SQLException {
    List<String> users = new ArrayList<>();
    String sql = "SELECT username FROM users WHERE active = ?";

    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        
        stmt.setBoolean(1, true);
        try (ResultSet rs = stmt.executeQuery()) {
            while (rs.next()) {
                users.add(rs.getString("username"));
            }
        }
    }
    return users;
}
```

---
*Back to [Samples](./)*
