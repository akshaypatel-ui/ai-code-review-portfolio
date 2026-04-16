# Sample Task: Python Ranking

**Prompt:** Create a Python function that reads a large CSV file and returns the average of a specific column, handling potential missing values.

---

## 🤖 Response A (The "Hallucinator")
```python
import pandas as pd

def get_average(file_path, column_name):
    df = pd.read_csv(file_path)
    return df[column_name].mean_with_nulls() # Error: phantom method
```

## 🤖 Response B (The "Sub-Optimal")
```python
import csv

def get_average(file_path, column_name):
    total = 0
    count = 0
    with open(file_path, 'r') as f:
        reader = csv.DictReader(f)
        for row in reader:
            total += float(row[column_name]) # Crashes on empty strings
            count += 1
    return total / count
```

## 🤖 Response C (The "Preferred")
```python
import pandas as pd

def get_average(file_path, column_name):
    try:
        df = pd.read_csv(file_path, usecols=[column_name])
        return df[column_name].dropna().mean()
    except (FileNotFoundError, ValueError, KeyError) as e:
        return f"Error: {e}"
```

---

## ⚖️ Evaluation & Ranking

**Rank: C > B > A**

### Justification:
- **Claim**: Response C is the most robust and efficient solution.
- **Evidence**: It uses `usecols` to minimize memory footprint during `read_csv` and includes comprehensive error handling for missing columns or files.
- **Implication**: In a production environment with multi-GB files, Response C prevents memory crashes and provides clear debug information.
- **Counter-Critique**: Response A is disqualified for using `mean_with_nulls()`, a non-existent Pandas method (Hallucination). Response B fails on files containing missing or non-numeric values as it lacks type validation.
