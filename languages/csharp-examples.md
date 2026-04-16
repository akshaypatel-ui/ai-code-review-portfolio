# C# & .NET Review Expertise

My .NET evaluation focus is on memory safety, deterministic resource management (IDisposable), and modern C# idioms (LINQ, Async/Await, and Nullable Reference Types).

## 🔍 Key Pitfalls I Check:
- **IDisposable Leaks**: Ensuring `using` statements or `using var` declarations are used for streams, database connections, and HTTP clients.
- **LINQ Performance**: Identifying expensive `IEnumerable` counts or unnecessary materialization (`.ToList()`) in loops.
- **Async/Await Antipatterns**: Spotting `Task.Result` or `.Wait()` which cause deadlocks, and encouraging `ConfigureAwait(false)` in library code.
- **Null Safety**: Auditing code for potential `NullReferenceException` risks, especially during serialization or database retrieval.

## 📂 Related Samples:
- [**C# Inventory Utility (Ranking)**](../samples/ranking-task-csharp.md): Focuses on resource leaks and null validation during JSON deserialization.

---
*Back to [Overview](../README.md)*
