# `pandas` Gists
A charcuterie board of `pandas` snippets that I can never seem to remember.


## Appending a DataFrame Dictionary
I frequently need to construct `pandas` DataFrames, and more often than not this includes: creating a dictionary to later convert to a DataFrame, iterating over an existing dictionary with my data, and appending the DataFrame dictionary. However, typing all of the `.append()` statements can be quite time-consuming, so here is a solution I came up with to expedite this process. In this example, I will use the first five [Mersenne](https://en.wikipedia.org/wiki/Mersenne_prime), [Good](https://en.wikipedia.org/wiki/Good_prime), and [Strong](https://en.wikipedia.org/wiki/Strong_prime) prime numbers as my data, where I need to construct a summary DataFrame that denotes the prime number class, minimum, maximum, and median of my data.

### Step 1. Construct the DataFrame Dictionary
I will admit, in this example, it wouldn't be *that* laborious to type everything out, but nevertheless.

```python
# Import packages.
import numpy as np
import pandas as pd

# Intialize a data dictionary.
data_dicc = {
    'Mersenne': np.array([2, 3, 5, 7, 13]),
    'Good': np.array([5, 11, 17, 29, 37]),
    'Strong': np.array([11, 17, 29, 37, 41]),
}
# Intialize a dataframe dictionary.
df_dicc = {
    'Prime Class': [],
    'Min': [],
    'Max': [],
    'Median': [],
}
```

### Step 2. Use `f-strings` to Print the `.append()` Statements
Here is a trick: after initializing the DataFrame dictionary, I simply format a string with all the `.append()` statements and print!

```python
# Intialize the text to print
to_print = '\n'.join(f"df_dicc['{key}'].append()" for key in df_dicc)
# Print the text you need to copy and paste.
print(to_print)
>>> df_dicc['Prime Class'].append()
>>> df_dicc['Min'].append()
>>> df_dicc['Max'].append()
>>> df_dicc['Median'].append()
```

### Step 3. Copy and Paste the `.append()` Statements
Lastly, I copy and paste all of the `.append()` statements to iteratively build the DataFrame dictionary.

```python
# For every key in the data dictionary.
for key in data_dicc:
    # Update the dictionary.
    df_dicc['Prime Class'].append(key)
    df_dicc['Min'].append(np.min(data_dicc[key]))
    df_dicc['Max'].append(np.max(data_dicc[key]))
    df_dicc['Median'].append(np.median(data_dicc[key]))
# Convert the dictionary to a dataframe.
df = pd.DataFrame(df_dicc)
# Show the dataframe.
print(df)
>>>   Prime Class  Min  Max  Median
>>> 0    Mersenne    2   13     5.0
>>> 1        Good    5   37    17.0
>>> 2      Strong   11   41    29.0
```


## Wide DataFrame $\rightarrow$ Long DataFrame
I was considering neighboring introgressed tracts per individual—i.e., two adjacent tracts—and wanted to sort all of the tracts in the DataFrame in descending order while keeping track (hahaha get it?) of the individual and tract ID. Unfortunately, I could not figure out a solution, but I did figure out the issue: my DataFrame was in a wide-format—i.e., one row per individual.

```python
# Import pandas.
import pandas as pd

# Intialize example data.
wide_df = pd.DataFrame({
    'IND': ['A', 'B', 'C'],
    'TRACT_LENGTH_1': [100, 150, 120],
    'TRACT_LENGTH_2': [200, 160, 100]
})
# Show the dataframe.
print(wide_df)
>>>   IND  TRACT_LENGTH_1  TRACT_LENGTH_2
>>> 0   A             100             200
>>> 1   B             150             160
>>> 2   C             120             100
```

It is not obvious to me that there is a straightforward way to sort by tract length while retaining the tract's individual and the tract length ID (`_1` or `_2`). However, I later realized that this task would become trivial if I converted my DataFrame to long-format—i.e., one row per individual per tract.

```python
# Elongate and sort the dataframe in descending order.
long_sorted_df = pd.melt(
    wide_df, id_vars='IND', value_vars=['TRACT_LENGTH_1', 'TRACT_LENGTH_2'],
    var_name='TRACT_LENGTH_ID', value_name='LENGTH',
).sort_values(by='LENGTH', ascending=False).reset_index(drop=True)
# Show the dataframe.
print(long_sorted_df)
>>>   IND TRACT_LENGTH_ID  LENGTH
>>> 0   A  TRACT_LENGTH_2     200
>>> 1   B  TRACT_LENGTH_2     160
>>> 2   B  TRACT_LENGTH_1     150
>>> 3   C  TRACT_LENGTH_1     120
>>> 4   A  TRACT_LENGTH_1     100
>>> 5   C  TRACT_LENGTH_2     100
```