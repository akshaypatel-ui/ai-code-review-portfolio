# Ranking Task: Modern C++ RAII vs. Raw Pointer Leaks

## 📝 The Prompt
> "An AI-generated C++ function reads integers from a file, filters positive values, and returns a dynamically allocated array. A tester reports that the program's memory usage grows without bound when the function is called repeatedly, and it crashes when given an empty file. Identify the bugs and provide a corrected version."

### 🚩 Original Code Snippet:
```cpp
int* processFile(const char* filename, int* outCount) {
    std::ifstream file(filename);
    if (!file.is_open()) {
        *outCount = 0;
        return nullptr;
    }
    // ... logic ...
    int* result = new int[count]; // Raw allocation
    return result;
}
```

---

## 🤖 Model Responses

### Response A
> There are **three bugs** here, all rooted in C-style patterns that violate modern C++ guidelines.
> 
> **Bug 1: Memory Leak — Raw `new` Without `delete`**
> The function allocates with `new int[count]` and returns the raw pointer. The caller never calls `delete[] data`.
> 
> **Bug 2: Null Pointer Dereference — Undefined Behavior**
> The function dereferences `outCount` without checking if it's `nullptr`.
> 
> **Fix — Return `std::vector` (RAII):**
> Use `std::vector<int>` to manage memory automatically. This adheres to **C++ Core Guidelines R.11**: "Avoid calling `new` and `delete` explicitly."

### Response B
> The caller needs to free the memory. Add `delete[]` after use:
> ```cpp
> void caller() {
>     int count;
>     int* data = processFile("data.txt", &count);
>     // ...
>     delete[] data;
> }
> ```

### Response C
> The memory leak is because you used `new` instead of `malloc`. Use `malloc` and `free` for C-style arrays.

---

## ⚖️ My Evaluation

**Ranking: Response A > Response B > Response C**

### 🧠 Justification

1.  **Fundamental Principles**: **Response A** correctly identifies the architectural flaw (raw owning pointers) and advocates for **RAII (Resource Acquisition Is Initialization)**.
2.  **Safety**: Response A is the only one to identify the **Null Pointer Dereference** on the `outCount` parameter, which is a major source of crashes.
3.  **Correctness**: Response B provides a manual fix that is error-prone and misses the null check. **Response C** provides dangerous advice, as `malloc/free` is an outdated pattern in C++ that bypasses type safety and constructor logic.

---
### 🛠 Gold Standard Correction
```cpp
#include <fstream>
#include <vector>

std::vector<int> processFile(const std::string& filename) {
    std::vector<int> result;
    std::ifstream file(filename);
    if (!file.is_open()) return result;

    int value;
    while (file >> value) {
        if (value > 0) result.push_back(value);
    }
    return result; // Managed by vector RAII
}
```

---
*Back to [Samples](./)*
