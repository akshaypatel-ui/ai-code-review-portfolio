# Ranking Task: C# Async Flow & LINQ Performance

## 📝 The Prompt
> "An AI-generated C# service fetches user data from an API and caches it. When the API throws an exception, the application crashes without any way to catch the error. Additionally, the cached data sometimes appears stale or re-fetches unnecessarily. Identify the bugs and provide a corrected version."

### 🚩 Original Code Snippet:
```csharp
public class UserService
{
    private IEnumerable<User> _cachedUsers;

    public async void RefreshUsersAsync()
    {
        var response = await _client.GetStringAsync("https://api.example.com/users");
        var users = ParseUsers(response);
        _cachedUsers = users.Where(u => u.IsActive);
    }

    public List<User> GetActiveUsers()
    {
        if (_cachedUsers == null) return new List<User>();
        return _cachedUsers.ToList();
    }
}
```

---

## 🤖 Model Responses

### Response A
> There are **two independent bugs** here — one about async exception handling, one about LINQ execution semantics.
> 
> **Bug 1: `async void` — Unhandled Exception Crash**
> `RefreshUsersAsync` is `async void`. In C#, the exception is thrown on the `SynchronizationContext` (e.g., ThreadPool) and becomes unhandled, causing a process crash. Callers cannot catch this.
> 
> **Bug 2: LINQ Deferred Execution — Storing a Query Instead of Results**
> `_cachedUsers = users.Where(u => u.IsActive)` stores a **query**, not the result. The filter runs every time `ToList()` is called in `GetActiveUsers`.
> 
> **Fix:**
> ```csharp
> public async Task RefreshUsersAsync()
> {
>     var response = await _client.GetStringAsync("https://api.example.com/users");
>     var users = ParseUsers(response);
>     _cachedUsers = users.Where(u => u.IsActive).ToList();
> }
> ```

### Response B
> Change `async void` to `async Task` so you can catch exceptions:
> ```csharp
> public async Task RefreshUsersAsync() { ... }
> ```
> `async void` methods can't be awaited, so exceptions are unhandled.

### Response C
> Wrap the body in try/catch to prevent the crash and add `.ToList()`:
> ```csharp
> public async void RefreshUsersAsync()
> {
>     try {
>         // ...
>         _cachedUsers = users.Where(u => u.IsActive).ToList();
>     } catch (Exception ex) { ... }
> }
> ```

---

## ⚖️ My Evaluation

**Ranking: Response A > Response C > Response B**

### 🧠 Justification

1.  **Technical Accuracy**: **Response A** provides a deep-dive into why `async void` crashes the process (SynchronizationContext behavior). It also correctly identifies the "cache" is essentially a lazy computation that re-executes on every call.
2.  **Architectural Best Practices**: Response A recommends `async Task`, which is the industry standard for service methods. **Response C** tries to "patch" the problem with a `try/catch` inside an `async void`, which is less optimal as callers still cannot track the task's completion.
3.  **Instruction Following**: **Response B** failed to identify the LINQ performance bug entirely, making it the weakest response despite having the correct fix for the async issue.

---
### 🛠 Gold Standard Correction
```csharp
public async Task RefreshUsersAsync()
{
    try 
    {
        var response = await _client.GetStringAsync("url");
        var users = ParseUsers(response);
        // Materialize immediately to prevent re-execution of the filter
        _cachedUsers = users.Where(u => u.IsActive).ToList();
    }
    catch (HttpRequestException ex)
    {
        // Log and handle the specific API failure
        _logger.LogError(ex, "Failed to refresh users cache.");
    }
}
```

---
*Back to [Samples](./)*
