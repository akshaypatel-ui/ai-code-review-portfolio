# Ranking Task: C# Resource Management & Null Handling

## 📝 The Prompt
> "An AI-generated C# utility reads configuration from a JSON file and returns a settings object. A tester reports that the application sometimes throws `NullReferenceException` and that file handles accumulate over time when the method is called repeatedly. Identify the bugs and provide a corrected version."

### 🚩 Original Code Snippet:
```csharp
public class ConfigLoader
{
    public Config Load(string path)
    {
        var stream = new FileStream(path, FileMode.Open, FileAccess.Read, FileShare.Read);
        var reader = new StreamReader(stream);
        var json = reader.ReadToEnd();
        var config = JsonSerializer.Deserialize<Config>(json);
        return config;
    }
}
```

---

## 🤖 Model Responses

### Response A
> There are **two bugs** here — a resource leak and a null reference — both extremely common in AI-generated C# code.
> 
> **Bug 1: Resource Leak — Missing `using` (IDisposable)**
> `FileStream` and `StreamReader` both implement `IDisposable`. They hold unmanaged resources (file handles). Without calling `Dispose()`, these handles remain open until the garbage collector eventually runs. Each call to `Load()` opens a handle that never closes.
> 
> **Bug 2: Null Reference — No Validation of Deserialized Result**
> `JsonSerializer.Deserialize<Config>(json)` returns `null` if the JSON content is `"null"`. Returning `null` from `Load()` means any caller doing `config.ServerUrl` gets a `NullReferenceException`.
> 
> **Fix:**
> ```csharp
> public Config Load(string path)
> {
>     using var stream = new FileStream(path, FileMode.Open, FileAccess.Read, FileShare.Read);
>     using var reader = new StreamReader(stream);
>     var json = reader.ReadToEnd();
>     var config = JsonSerializer.Deserialize<Config>(json);
> 
>     if (config == null)
>         throw new InvalidOperationException($"Failed to deserialize config from {path}");
> 
>     return config;
> }
> ```

### Response B
> The streams are not being closed. Add `using` statements:
> ```csharp
> public Config Load(string path)
> {
>     using var stream = new FileStream(path, FileMode.Open, FileAccess.Read, FileShare.Read);
>     using var reader = new StreamReader(stream);
>     var json = reader.ReadToEnd();
>     var config = JsonSerializer.Deserialize<Config>(json);
>     return config;
> }
> ```

### Response C
> The problem is that you need to close the file after reading. Add `Close()` calls:
> ```csharp
> public Config Load(string path)
> {
>     var stream = new FileStream(path, FileMode.Open, FileAccess.Read, FileShare.Read);
>     var reader = new StreamReader(stream);
>     var json = reader.ReadToEnd(); ...
>     reader.Close();
>     stream.Close();
>     return config;
> }
> ```

---

## ⚖️ My Evaluation

**Ranking: Response A > Response B > Response C**

### 🧠 Justification

1.  **Root Cause Identification**: The prompt highlights two issues (`IOException` and `NullReferenceException`). Only **Response A** identifies the second root cause—the failure to validate the deserialization result.
2.  **Deterministic Management**: Response A correctly uses the `using var` pattern. **Response C** suggests manual `.Close()` calls, which is an anti-pattern because it fails to release handles if an exception occurs during the read operation.
3.  **Exception Safety**: Response A's explanation of how `IDisposable` works (noting that the GC does *not* call `Dispose()`) provides critical educational context for why the `using` pattern is mandatory for production-grade .NET applications.

---
### 🛠 Gold Standard Correction
```csharp
public Config Load(string path)
{
    if (!File.Exists(path))
        throw new FileNotFoundException($"Config file not found at {path}");

    var json = File.ReadAllText(path);
    return JsonSerializer.Deserialize<Config>(json) 
           ?? throw new InvalidOperationException("Config deserialization failed.");
}
```

---
*Back to [Samples](./)*
