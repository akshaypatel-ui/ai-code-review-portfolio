# AI Code Evaluation Methodology

High-quality AI training requires more than just "correcting" code; it requires structured feedback that helps the model learn the *reasoning* behind excellence. I follow a rigorous 3-pass review methodology.

## 🛡 High-Level Prioritization
1. **Safety & Security**: Does the code contain critical vulnerabilities?
2. **Correctness**: Does the code solve the prompt's requirements without logic flaws?
3. **Efficiency**: Is the algorithmic complexity (Big O) optimized for scale?
4. **Maintainability**: Is the code idiomatic, readable, and well-structured?

---

## 🔍 The 3-Pass Review Approach

### Pass 1: Static Analysis & Logic Verification
- **Tracing**: Manual code trace to replicate the model's logic.
- **Boundary Check**: Identifying potential null-pointers, off-by-one errors, and unhandled edge cases.
- **Correctness Check**: Verifying if the AI "hallucinated" a library function that doesn't exist.

### Pass 2: Security & Performance Audit
- **Security**: Checking for common injection risks or unsafe data handling.
- **Big O Analysis**: Evaluating time and space complexity to ensure the response isn't just correct, but efficient.

### Pass 3: Feedback Engineering
- **Refinement**: Applying "minimal necessary changes" to preserve model intent while enforcing standard patterns.
- **Justification**: Using the **Claim → Evidence → Implication** framework:
    - **Claim**: Precise identification of an issue/strength.
    - **Evidence**: Specific line number or logic walk-through.
    - **Implication**: What happens if this code runs in production?
    - **Action**: How the model should improve.

---

## 🧪 Justification Quality Standards

Every review in this portfolio adheres to the following standards:
- **No Circular Logic**: I don't say "this is bad because it's not good."
- **Actionable Evidence**: I cite specific lines or logical contradictions.
- **Professional Tone**: Objective, precise, and constructive feedback.
