# Sample Task: Multi-Language Ranking (Binary Search)

**Prompt:** Implement a binary search function: given a sorted array and a target, return the index of the target or -1 if not found.

---

## 🤖 Response A (Python)
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

## 🤖 Response B (Java)
```java
public static int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2; // Optimization
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

## 🤖 Response C (Kotlin)
```kotlin
fun binarySearch(arr: IntArray, target: Int): Int {
    var left = 0
    var right = arr.size - 1
    while (left <= right) {
        val mid = (left + right) / 2
        when {
            arr[mid] == target -> return mid
            arr[mid] < target -> left = mid + 1
            else -> right = mid - 1
        }
    }
    return -1
}
```

---

## ⚖️ Evaluation & Ranking

**Rank: B > A > C**

### Justification:
- **Claim**: Response B is the most robust implementation due to its handling of integer overflow, whereas Response C contains a potential runtime bug for extremely large datasets.
- **Evidence**: 
    - **In Response B (Java)**: The calculation `mid = left + (right - left) / 2` prevents overflow if `left + right` exceeds `Integer.MAX_VALUE`.
    - **In Response C (Kotlin)**: The calculation `mid = (left + right) / 2` causes integer overflow when the sum of indices exceeds $2^{31}-1$, leading to a negative `mid` and an `ArrayIndexOutOfBoundsException`.
    - **In Response A (Python)**: While `(left + right) // 2` is safe because Python handles arbitrary-precision integers, it is less "transferable" as a pattern compared to the optimized formula in B.
- **Implication**: On modern systems with massive arrays (e.g., memory-mapped files), Response C's flaw can lead to catastrophic failure, while Response B remains reliable.
- **Actionable**: Response C and A should be updated to use the overflow-safe midpoint formula to ensure cross-language consistency and production safety.

---
*This evaluation demonstrates cross-language awareness of primitive type limitations and memory safety.*
