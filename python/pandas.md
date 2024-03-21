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