
# 📚 Handling Dates and Times in Pandas: Timestamp, DatetimeIndex, Period

Pandas provides several functions to handle date and time-related data.

Objects like `Timestamp`, `DatetimeIndex`, and `Period` are very useful for processing and analyzing date and time data. In this post, we’ll explore the main features of these objects.

---

## 📚 Timestamp

A `Timestamp` represents a single point in time in Pandas.

It's similar to `datetime64` but offers more convenient features. `Timestamp` objects can be created with various units, supporting nanoseconds (`ns`) by default.

### Example 1: Using a single value as a date

```python
import pandas as pd

example1 = pd.Timestamp(1234.5678)  # Default is nanoseconds
example2 = pd.Timestamp(1234.5678, unit='D')  # Days unit

print(example1)
print(example2)
```

**Output:**

```
1970-01-01 00:00:00.000001234
1973-05-19 13:37:37.920000
```

- `example1`: Interprets 1234.5678 in nanoseconds from 1970-01-01.
- `example2`: Interprets 1234.5678 in days from 1970-01-01.

---

## 📚 DatetimeIndex

`DatetimeIndex` is useful when handling multiple dates. It allows managing multiple dates in a single index.

### Example 2: Converting single and multiple dates to `DatetimeIndex`

```python
import pandas as pd

example1 = pd.to_datetime('2023-11-1 12')  # Single date
example2 = pd.to_datetime(['2023-1-1', '2023-10-31'])  # Multiple dates

print(example1)
print(example2)
```

**Output:**

```
2023-11-01 12:00:00
DatetimeIndex(['2023-01-01', '2023-10-31'], dtype='datetime64[ns]', freq=None)
```

- `example1`: Converted to a single datetime.
- `example2`: Converted to a `DatetimeIndex` object.

---

### Example 3: Automatically generating a range of dates

Use `pd.date_range()` to generate dates over a specified period.

```python
import pandas as pd

example = pd.date_range('2023-1-1', '2023-10-31')
print(example)
```

**Output:**
Shows 304 daily dates from January 1 to October 31, 2023.

---

## 📚 Period and PeriodIndex

`Period` represents a span of time rather than a specific moment. For example, a month or a year. In contrast, `Timestamp` represents a specific point in time.

### Example 4: Creating `Period` objects

```python
import pandas as pd

example1 = pd.Period('2023-11-1')  # Default unit is 'D'
example2 = pd.Period('2023-11-1', freq='M')  # Monthly
example3 = pd.Period('2023-11-1', freq='Y')  # Yearly

print(example1)
print(example2)
print(example3)
print(type(example1))
```

**Output:**

```
2023-11-01
2023-11
2023
<class 'pandas._libs.tslibs.period.Period'>
```

- `example1`: Interpreted as a day.
- `example2`: Interpreted as a month.
- `example3`: Interpreted as a year.

---

### Example 5: Creating a range of `Period` values

Use `pd.period_range()` to generate a series of `Period` objects.

```python
import pandas as pd

example1 = pd.period_range('2023-1', '2023-11')  # Monthly by default
example2 = pd.period_range('2023-1', '2023-11', freq='M')  # Monthly
example3 = pd.period_range('2022-1', '2023-11', freq='Y')  # Yearly

print(example1)
print(example2)
print(example3)
print(type(example1))
```

**Output:**

```
PeriodIndex(['2023-01', '2023-02', ..., '2023-11'], dtype='period[M]', freq='M')
PeriodIndex(['2023-01', '2023-02', ..., '2023-11'], dtype='period[M]', freq='M')
PeriodIndex(['2022', '2023'], dtype='period[Y]', freq='Y')
<class 'pandas._libs.tslibs.period.Period'>
```

- `example1`: Monthly periods from January to November 2023.
- `example2`: Same as above.
- `example3`: Yearly periods for 2022 and 2023.
