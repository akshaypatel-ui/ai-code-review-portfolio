# JSON & Schema Expertise

In the context of AI evaluation, reviewing JSON is about ensuring data integrity, schema compliance, and injection safety. I specialize in auditing how models handle structured data exchange.

## 🔍 Key Pitfalls I Check:
- **Schema Compliance**: Verifying if the JSON output matches a required JSON Schema (e.g., draft-07).
- **Data Type Type-Safety**: Identifying issues where a model uses strings instead of numbers, or fails to handle null values correctly.
- **Security (Injection)**: Checking if user-provided strings inside JSON are properly escaped to prevent subsequent injection in the processing pipeline.
- **Syntax Correctness**: Identifying trailing commas, unquoted keys, or mismatched brackets that break parsers.
    - *Example*: Spotting a trailing comma which is valid in JS but invalid in strict JSON.

## 📂 Related Samples:
- [**JSON Ranking Task**](../samples/ranking-task-json.md): Evaluation of schema adherence and nested logic validation.
- [**Web Security Scoring**](../samples/scoring-task-1.md): Focuses on how JSON payloads are safely handled in a Node.js environment.

---
*Back to [Overview](../README.md)*
