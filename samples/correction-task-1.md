# Sample Task: Correction (Java Logic)

**Prompt:** Fix the error in this Java method intended to calculate the factorial of a number using recursion.

---

## 🤖 Buggy AI Response
```java
public int factorial(int n) {
    if (n == 1) {
        return 1;
    }
    return n * factorial(n); // Infinite recursion
}
```

---

## ✅ Corrected Solution (Gold Standard)
```java
public int factorial(int n) {
    // Base Case: 0! and 1! are 1
    if (n <= 1) {
        return 1;
    }
    // Corrected reduction: n-1
    return n * factorial(n - 1);
}
```

### Explanation of Changes:
1. **Recursion Depth**: Changed `factorial(n)` to `factorial(n - 1)`. 
   - **Reasoning**: Recursive calls must move toward a base case to avoid a `StackOverflowError`. 
2. **Base Case Safety**: Changed `n == 1` to `n <= 1`.
   - **Reasoning**: Prevents infinite negative recursion if the user inputs `0` or a negative number.
3. **Complexity Note**: While functional, for extremely large `n`, an iterative approach or `BigInteger` should be used to avoid integer overflow.
