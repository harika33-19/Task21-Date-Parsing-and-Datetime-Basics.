# Date Parsing and Datetime Basics — Python + Pandas

## Objective
Convert text dates into Pandas datetime values, extract year/month/day/weekday, handle invalid dates, and perform a short date-based analysis.

## Tools
- Python
- Pandas
- Jupyter Notebook

## 1. Import Pandas

```python
import pandas as pd
```

Pandas helps us work with data arranged in rows and columns.

## 2. Convert text into datetime

```python
import pandas as pd

dates = pd.Series(["2026-01-15", "2026-05-20", "2026-10-21"])
dates = pd.to_datetime(dates)

print(dates)
print(dates.dtype)
```

`pd.to_datetime()` converts date text into datetime values.

## 3. Extract year, month, day, and weekday

```python
import pandas as pd

df = pd.DataFrame({
    "Date": ["2026-01-15", "2026-05-20", "2026-10-21"]
})

df["Date"] = pd.to_datetime(df["Date"])
df["Year"] = df["Date"].dt.year
df["Month"] = df["Date"].dt.month
df["Day"] = df["Date"].dt.day
df["Weekday"] = df["Date"].dt.day_name()

print(df)
```

- `.dt.year` extracts the year.
- `.dt.month` extracts the month number.
- `.dt.day` extracts the day of the month.
- `.dt.day_name()` gives the weekday name.

## 4. Handle invalid dates

```python
import pandas as pd

dates = pd.Series(["2026-01-15", "not-a-date", "2026-13-40"])
converted_dates = pd.to_datetime(dates, errors="coerce")

print(converted_dates)
print("Invalid date count:", converted_dates.isna().sum())
```

`errors="coerce"` changes invalid dates to `NaT`, Pandas' missing-date value.

## 5. Convert dates in day/month/year format

```python
import pandas as pd

dates = pd.Series(["15/01/2026", "20/05/2026", "21/10/2026"])
dates = pd.to_datetime(dates, format="%d/%m/%Y")
print(dates)
```

Format codes: `%d` = day, `%m` = month, `%Y` = four-digit year.

## 6. Find the earliest and latest date

```python
import pandas as pd

df = pd.DataFrame({
    "OrderDate": ["2026-02-10", "2026-01-05", "2026-03-25", "2026-02-18"]
})
df["OrderDate"] = pd.to_datetime(df["OrderDate"])

print("Earliest date:", df["OrderDate"].min())
print("Latest date:", df["OrderDate"].max())
```

## 7. Sort rows by date

```python
import pandas as pd

df = pd.DataFrame({
    "OrderDate": ["2026-03-25", "2026-01-05", "2026-02-10"],
    "Sales": [300, 100, 200]
})
df["OrderDate"] = pd.to_datetime(df["OrderDate"])
df = df.sort_values("OrderDate")
print(df)
```

## 8. Filter rows after a date

```python
import pandas as pd

df = pd.DataFrame({
    "OrderDate": ["2026-01-05", "2026-02-10", "2026-03-25"],
    "Sales": [100, 200, 300]
})
df["OrderDate"] = pd.to_datetime(df["OrderDate"])

filtered = df[df["OrderDate"] >= "2026-02-01"]
print(filtered)
```

## 9. Count records by weekday

```python
import pandas as pd

df = pd.DataFrame({
    "Date": ["2026-01-15", "2026-01-16", "2026-01-17", "2026-01-18"]
})
df["Date"] = pd.to_datetime(df["Date"])
df["Weekday"] = df["Date"].dt.day_name()

print(df["Weekday"].value_counts())
```

## 10. Calculate days between two dates

```python
import pandas as pd

start_date = pd.to_datetime("2026-01-01")
end_date = pd.to_datetime("2026-01-21")

difference = end_date - start_date
print("Number of days:", difference.days)
```

## 11. Complete mini-project: date analysis

Run this complete program to convert dates, detect invalid values, create new columns, and summarize sales.

```python
import pandas as pd

df = pd.DataFrame({
    "OrderDate": ["2026-01-15", "2026-02-20", "invalid", "2026-01-25", "2026-03-05"],
    "Sales": [1200, 800, 500, 1500, 950]
})

# Convert date text; invalid values become NaT
df["OrderDate"] = pd.to_datetime(df["OrderDate"], errors="coerce")

# Count invalid dates
invalid_count = df["OrderDate"].isna().sum()
print("Invalid dates:", invalid_count)

# Remove invalid dates before analysis
df = df.dropna(subset=["OrderDate"]).copy()

# Extract date parts
df["Year"] = df["OrderDate"].dt.year
df["Month"] = df["OrderDate"].dt.month
df["Day"] = df["OrderDate"].dt.day
df["Weekday"] = df["OrderDate"].dt.day_name()

# Sort by date
df = df.sort_values("OrderDate")
print("\\nCleaned data:")
print(df)

print("\\nTotal sales:", df["Sales"].sum())
print("Earliest order:", df["OrderDate"].min().date())
print("Latest order:", df["OrderDate"].max().date())

print("\\nSales by month:")
print(df.groupby("Month")["Sales"].sum())
```

### What the mini-project does
1. Converts date strings into datetime values.
2. Counts invalid dates.
3. Removes invalid dates before date analysis.
4. Creates Year, Month, Day, and Weekday columns.
5. Sorts dates and calculates total sales and sales by month.

## 12. Practice questions
1. Convert three date strings to datetime.
2. Extract the month and weekday from a date column.
3. Find invalid dates in a list.
4. Sort a DataFrame from earliest to latest date.
5. Filter records after `2026-02-01`.

## Interview questions and answers

**Q1. How do you convert a column to datetime?**  
Use `df["Date"] = pd.to_datetime(df["Date"])`.

**Q2. What is the `.dt` accessor?**  
It lets you access parts of datetime values, such as year, month, day, and weekday.

**Q3. How can you handle invalid date values?**  
Use `pd.to_datetime(column, errors="coerce")`. Invalid values become `NaT`.

**Q4. How do you extract the weekday name?**  
Use `df["Date"].dt.day_name()`.

## How to run
1. Open Jupyter Notebook or another Python environment.
2. If Pandas is not installed, run `pip install pandas` in your terminal.
3. Copy one code example into a notebook cell and run it.
4. Compare your output with the example results.

## Summary
- `pd.to_datetime()` converts text to datetime.
- `.dt.year`, `.dt.month`, and `.dt.day` extract date parts.
- `.dt.day_name()` returns the weekday.
- `errors="coerce"` converts invalid dates to `NaT`.

```


