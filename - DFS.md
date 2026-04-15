```dataview
TABLE
FROM #dfs  
SORT file.name Asc
```

```python
import pandas as pd

# Load the dataset
df = pd.read_csv("messy_customers.csv")

# 1. Detect Invalid Email Formats [cite: 26]
# Valid emails must have text, an @ symbol, text, a dot, and text.
df["email_valid"] = df["email"].str.contains(r"^[^\s@]+@[^\s@]+\.[^\s@]+$", na=False) [cite: 27]
email_invalid_pct = (~df["email_valid"]).mean() * 100

# 2. Detect Invalid Ages [cite: 28]
# Valid ages should reasonably be between 0 and 120.
df["age_valid"] = pd.to_numeric(df["age"], errors="coerce").between(0, 120) [cite: 29]
age_invalid_pct = (~df["age_valid"]).mean() * 100

# 3. Detect Future Signup Dates
# Dates occurring after the current year are invalid.
df['signup_date_parsed'] = pd.to_datetime(df['signup_date'], errors='coerce', format='mixed')
future_dates_pct = (df['signup_date_parsed'].dt.year > 2026).mean() * 100

print(f"Percentage of invalid emails: {email_invalid_pct:.2f}%")
print(f"Percentage of invalid ages: {age_invalid_pct:.2f}%")
print(f"Percentage of future signup dates: {future_dates_pct:.2f}%")
```