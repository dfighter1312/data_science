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
```python
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

### 3. Uniqueness constraints

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

## Chapter 2: Text and categorical data problems

### 1. Membership constraints

**Why?**: Data Entry Errors, Parsing Errors

**How do we treat?**
- Dropping data
- Remapping categories
- Inferring categories

```python
# We have study_data DataFrame with blood_type and categories DataFrame store all possible blood_type values.
# Recall inner join and anti joins

inconsistent_categories = set(study_data['blood_type']).difference(categories['blood_type'])

# Get and print rows ith inconsistent categories
inconsistent_rows = study_data['blood_type'].isin(inconsistent_categories)
study_data[inconsistent_rows]

# Dropping inconsistent categories
consistent_data = study_data[~inconsistent_rows]
```

### 2. Categorical variables

**What type of errors could we have?**

#### Value inconsistency

- Capitalization: use `.str.upper()`, `.str.lower()`
- Trailing spaces: use `.str.strip()`

#### Collapsing too many categories to few

- Create categories out of data
```python
ranges = [0, 200000, 500000, np.inf]
group_names = ['0-200K', '200K-500K', '500K+']

# Create income group column
df['income_group'] = pd.cut(df['household_income'], bins=ranges, labels=group_names)
```

- Map categories to fewer ones:
```python
mapping = {'11':'1', '12':'1', '13':'1', '21':'2', '22':'2'}
df['label'] = df['label'].replace(mapping)
```

#### Making sure data is of type `category`

### 3. Cleaning text data

- Data inconsistency
- Fixed length violations
- Typos

Note: Assert all numbers do not have "+" or "-"
```python
assert phone['Phone number'].str.contains('+|-').any() == False
```

Regular expression
```python
phones['Phone number'] = phones['Phone number'].str.replace(r'\D+', '')
```

## Chapter 3: Advanced data problems

### 1. Uniformity

- Metric conversion
- Date Time format
```python
# Convert account_opened to datetime
banking['account_opened'] = pd.to_datetime(banking['account_opened'],
                                           # Infer datetime format
                                           infer_datetime_format = True,
                                           # Return missing value for error
                                           errors = 'coerce') 
# Get year of account opened
banking['acct_year'] = banking['account_opened'].dt.strftime('%Y')
```

### 2. Cross field validation 

The use of **multiple** fields in a dataset to sanity check data integrity.

**What to do when we catch inconsistencies?**
- Dropping data
- Set to missing and impute
- Apply rules from domain knowledge

### 3. Completeness

One of the most common problems.

Can be represented as `NA`, `nan`, `0`, `.`

Caused by:
- Technical error
- Human error

**How to find missing values?**

Use `.isna()`, `.isna().sum()`

Use `missingno` package

```python
import missingno as msno
import matplotlib.pyplot as plt

# Visualize missingness
msno.matrix(df)
plt.show()
```

**Missingness types**
- Missing completely at random (MCAR): No systematic relationship between missing data and other vlaues. Data entry errors when inputting data.
- Missing at random (MAR): Systematic relationship between missing dataa and other <span style="color:red;">observed</span> values. Eg: Missing ozone data for high temperatures.
- Missing Not at Random (MNAR): Systematic relationship between missing data and <span style="color:red;">unobserved</span> values. Eg: Missing temperature values for high temperatures.

**How to deal with misisng data?**
- Simple approaches:
  - Drop missing data
  - Impute with statistical measures *(mean, median, mode..)*
- More complex approaches:
  - Imputing using an algorithmic approach
  - Impute with machine learning models

## Chapter 4: Record linkage

### 1. Comparing strings

**Why?**: to know whether there is a typo or not.

**Minimum edit distance**: Least possible amount of steps needed to transition from one string to another. Eg: *reading* and *redaing* has minimum edit distance = 1.

Possible packages: `ntlk`, `fuzzywuzzy`, `textdistance`

```python
# Compare between two strings
from fuzzywuzzy import fuzz

# Compare reeding vs reading
fuzz.Wratio('Reeding', 'Reading')
```

Output:
```86```
The output is comparison score from 0 to 100, with 0 being not similar at all, 100 being an exact match.

**Comparison with arrays**
```python
from fuzzywuzzy import process

# Define string and array of possible matches
string = "Houston Rockets vs Los Angelies Lakers"
choice = pd.Series(['Rockets vs Lakers', 'Lakers vs Rockets',
                    'Houson vs Los Angeles', 'Heat vs Bulls'])
                    
process.extract(string, choices, limit=2)
```

Output
```[('Rockets vs Lakers', 86, 0), ('Lakers vs Rockets', 86, 1)]```

**Collapsing categories with string matching**
```python
for state in categories['state']:
  # Find potential matches in states with typoes
  matches = process.extract(state, survey['state'], limit=survey.shape[0])
  # For each potential match match
  for potential_match in matches:
    # If high similarity score
    if potential_match[1] >= 80:
        # Replace typo with correct category
        survey.loc[survey['state'] == potential_match[0], 'state'] = state
```

### 2. Generating pairs

**Record linkage**: the cat of linking data from different sources regarding the same entity.

Steps:
- Generate pairs
- Compare pairs
- Score pairs
- Link data

Use `recordlinkage` package.

Example: 2 dataframes `census_A` and `census_B`.
```python
import recordlinkage

# Create indexing object
indexer = recordlinkage.Index()

# Generate pairs blocked on state
indexer.block('state')
pairs = indexer.index(census_A, census_B)

# Create a Compare object
compare_cl = recordlinkage.Compare()

# Find exact matches for pairs of date_of_birth and state
compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
compare_cl.exact('state', 'state', label='state')

# Find similar matches for pairs
compare_cl.string('surname', 'surname', threshold=0.85, label='surname')

# Find matches
potential_matches = compare_cl.compute(pairs, census_A, census_B)
```


### 3. Linking DataFrames

```python
# Find good matches
matches = potential_matches[potential_matches.sum(axis=1) >= 3]

# Get indices from census_B only
duplicate_rows = matches.index.get_level_values(1)
print(census_B_index)
```

Output:
```Index(['rec-2404-dup-0', 'rec-4178-dup-0', ... , 'rec-299-dup-0'])```

```python
# Finding duplicates in census_B
census_B_duplicates = census_B[census_B.index.isin(duplicate_rows)]

# Link the DataFrame
full_census = census_A.append(census_B_new)
```



