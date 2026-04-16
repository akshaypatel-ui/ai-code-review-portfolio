# Python Review Expertise

My Python evaluation methodology focuses on PEP 8 compliance, idiomatic "Pythonic" patterns, and the safe usage of popular libraries like Pandas and NumPy.

## 🔍 Key Pitfalls I Check:
- **Mutable Default Arguments**: Identifying `def func(x=[])` bugs.
- **Pandas Efficiency**: Spotting row-by-row iteration (`iterrows`) vs. vectorization.
- **Memory Management**: Identifying $O(n^2)$ string concatenation in loops.
- **Type Hinting**: Encouraging the use of `typing` for clearer contract definition.

## 📂 Related Samples:
- [**Python Ranking (CSV Processing)**](../samples/ranking-task-1.md): Focuses on library hallucinations and memory safety.
- [**Performance Scoring**](../samples/scoring-task-2.md): Analysis of string concatenation efficiency.
- [**Multi-Language Binary Search**](../samples/ranking-task-2.md): Analysis of type-safety and standard algorithms.
