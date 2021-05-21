# Cleaning data in Python

## Chapter 1: Common data problem

#### Why do we need to clean data?

Dirty data can appear because of duplicate values, mis-spellings, data type parsing errors and legacy systems.
Without making sure that data is properly cleaned in the exploration and processing phase, 
we will surely compromise the insights and reports subsequently generated.

### 1. Data type constraints

Python data type: `str`, `int`, `float`, `bool`, `datetime`, `category`.

Strings to integers

```python
# Import CSV file and output header
sales = pd.read_csv('sales.csv')
sales.dtypes
```
Output:
```
SalesOrderID  int64
Revenue       object
Quantity      int64
dtype: object
```

```python
# Print sum of all Revenue column
sales['Revenue'].sum()
```
Output:
```
'23153$1457$36865$32474$472$27510$16158$5694$6876$40487$807$6893$9153$6895$4216..
```
# Remove $ from Revenue column
sales['Revenue'] = sales['Revenue'].str.strip('$')
sales['Revenue'] = sales['Revenue'].astype('int')

# Verify that Revenue is now an integer
assert sales['Revenue'].dtype == 'int'

```

#### The assert statement
```python
assert 1+1 == 2   #pass
assert 1+1 == 3   #raise AssertionError
```

#### Numeric or categorical?

Suppose `marriage_status` with a number represents a status:
- `0` = Never married
- `1` = Married
- `2` = Seperated
- `3` = Divoced

=> `categorical` data type.

```python
# Convert to categorical
df["marriage_status"] = df["marriage_status"].astype('category')
df.describe()
```

### 2. Data range constraints

Why dealing?
- Movie rating can be an integer around 1 to 5
- The subscription date cannot be greater than `dt.date.today()`

How to deal with out of range data?
- Dropping data
- Setting custom min and max
- Treat as missing and impute
- Setting custom value depending on business assumptions

#### Movie example
```python
import pandas as pd
# Output Movies with rating > 5
movies[movies['avg_rating'] > 5]

# Drop values using filtering
movies = movies[movies['avg_rating'] <= 5]
# Drop values using .drop()
movies.drop(movies[movies['avg_rating'] > 5].index, inplace=True)
# Assert results
assert movies['avg_rating'].max() <= 5

# Convert avg_rating > 5 to 5
movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = 5
# Assert results
assert movies['avg_rating'].max() <= 5    # No output mean it passed
```

#### Date range example
```python
# Convert object to DateTime
df['date'] = pd.to_datetime(df['date'])
# Assert that conversion happened
assert df['date'].dtype == 'datetime64[ns]'

today_date = dt.date.today()
# Drop values using .drop()
df.drop(df[df['date'] > today_date].index, inplace=True)
# Assert statement
assert df.date.max().date() <= today_date
```

### Uniqueness constraints

#### What are dupplicate values?

Same rows across all columns, except 1-2 column(s).
**Why**: Data Entry & Human Error, Bugs, Design, Join or merge errors

```python
# Find duplicate value across all columns
duplicates = df.duplicated()
```
Output (boolean Series):
```
1   False
2   False
...
101 True
102 True
103 False
```
```python
# Get duplicate values
df[duplicates]
```

Arguments of `.duplicated()`
- `subset`: List of column names to check for duplication.
- `keep`: Whether to keep `first`, `last` or `False` (all) duplicate values.

Drop duplicated rows with `.drop_duplicates()` method:
- `subset`: List of column names to check for duplication.
- `keep`: Whether to keep `first` (as default), `last` or `False` (all) duplicate values.
- `inplace`: Drop duplicated rows directly inside DataFrame without creating new object (`True`).

Group by a set of common columns and return statistical values for specific
columns when the aggregation is being performed.
Use `.groupby()` and `.agg()`

```python
column_names = ['first_name', 'last_name', 'address']
summaries = {'height': 'max', 'weight': 'mean'}
height_weight = height_weight.groupby(by=column_names).agg(summaries).reset_index()
```





