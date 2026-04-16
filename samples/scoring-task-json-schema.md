# Scoring Task: JSON Schema & Required Fields

## 📝 The Prompt
> "Evaluate a model response that generates a user profile JSON. The profile must strictly follow this schema:
> - `id`: UUID (required)
> - `username`: string (required)
> - `email`: string (required)
> - `bio`: string (optional)"

---

## 🤖 Model Response
```json
{
  "username": "coder_99",
  "email": "coder99@example.com",
  "bio": "Exploring the world of AI."
}
```

---

## ⚖️ My Score: 2/5 (Failure)

### 🧠 Justification

#### 1. Missing Required Field (Major Issue)
*   **Observation**: The model failed to include the `id` field.
*   **Evidence**: The prompt explicitly stated `id: UUID (required)`.
*   **Implication**: In a production database, this JSON would fail a "NOT NULL" constraint check or a schema validation middleware, causing the application to crash or reject the record. Returning a payload that misses a mandatory Primary Key is a critical failure.

#### 2. Pattern Matching
*   **Observation**: While the `email` and `username` fields are present and valid strings, the failure to generate a valid `UUID` for the `id` shows a lack of attention to data format constraints.

#### 3. Rubric Breakdown:
*   **Correctness**: 1/5 (Core requirement missed).
*   **Instruction Following**: 1/5 (Ignored the 'required' list).
*   **Formatting**: 5/5 (Syntax is valid JSON).

---
### 🛠 Correction (Gold Standard)
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "username": "coder_99",
  "email": "coder99@example.com",
  "bio": "Exploring the world of AI."
}
```

---
*Back to [Samples](./)*
