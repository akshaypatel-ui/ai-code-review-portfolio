# C++ & System Programming Expertise

My C++ evaluation methodology is centered on Modern C++ (C++11/14/17/20) standards, emphasizing memory safety through RAII and the elimination of manual resource management.

## 🔍 Key Pitfalls I Check:
- **Ownership Ambiguity**: Identifying raw pointers used for ownership and recommending `std::unique_ptr`, `std::shared_ptr`, or value types (`std::vector`).
- **RAII Compliance**: Ensuring files, sockets, and memory are managed by object lifetimes to prevent resource leaks during exceptions.
- **Undefined Behavior (UB)**: Spotting null pointer dereferences, out-of-bounds access, and uninitialized variables.
- **Resource Efficiency**: Moving from C-style arrays to `std::vector` and utilizing move semantics to prevent unnecessary deep copies.

## 📂 Related Samples:
- [**C++ File Processor (Ranking)**](../samples/ranking-task-cpp.md): Evaluation of raw pointer memory leaks vs. modern RAII patterns.

---
*Back to [Overview](../README.md)*
