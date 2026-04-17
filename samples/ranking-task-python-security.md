# Ranking Task: Python Web Security & SQL Injection

## 📝 The Prompt
> "Review this Python function and suggest improvements:
> ```python
> def get_user(id):
>     result = db.query(f"SELECT * FROM users WHERE id = {id}")
>     return result[0]
> ```"

---

## 🤖 Model Responses

### Response A
> There are **several issues** with this function:
> 
> **1. SQL Injection Vulnerability (Critical)**
> Using f-strings for SQL queries allows SQL injection attacks. Use parameterized queries instead.
> 
> **2. No Error Handling**
> If the query returns no results, `result[0]` will raise an IndexError.
> 
> **3. Using SELECT ***
> This fetches unnecessary columns and exposes your schema.
> 
> **Improved version:**
> ```python
> def get_user(user_id: int) -> dict | None:
>     try:
>         result = db.query(
>             "SELECT id, name, email FROM users WHERE id = %s",
>             (user_id,)
>         )
>         return result[0] if result else None
>     except DatabaseError as e:
>         logger.error(f"Failed to fetch user {user_id}: {e}")
>         raise
> ```

### Response B
> The function has a bug - you should add a try/except:
> ```python
> def get_user(id):
>     try:
>         result = db.query(f"SELECT * FROM users WHERE id = {id}")
>         return result[0]
>     except:
>         return None
> ```

---

## ⚖️ My Evaluation

**Ranking: Response A > Response B**

### 🧠 Justification

1.  **Security Awareness**: **Response A** correctly identifies the **SQL Injection** vulnerability as a critical issue. Response B fails to notice the security risk entirely, which is a significant oversight for an AI evaluator.
2.  **Code Quality**: Response A provides a clean, parameterized fix with type hints and renames the shadowed `id` built-in. **Response B** suggests a **bare `except:` block**, which is a Python anti-pattern that can lead to silent failures and difficult debugging.
3.  **Completeness**: Response A addresses column selection hygiene (`SELECT *`), ensuring the code is not just secure but also efficient.

---
### 🛠 Gold Standard Correction
```python
def get_user(user_id: int) -> dict | None:
    # Use parameterized queries to prevent SQL Injection
    query = "SELECT id, username, email FROM users WHERE id = %s"
    result = db.query(query, (user_id,))
    
    # Return None if no user is found instead of crashing
    return result[0] if result else None
```

---
*Back to [Samples](./)*
