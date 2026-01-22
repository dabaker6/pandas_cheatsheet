# Pandas cheatsheet

Installing pandas and checking version

conda install pandas

import pandas as pd
import numpy as np
import re

Check version of pandas: pd.__version__

Loading dataframes

df = pd.read_[DATA SOURCE TYPE] where data source type is csv, json, excel etc...
- use_cols
- dtypes
- index
- 
df = (
    pd.read_csv(
    filename,
    sep='\t',
    usecols=['Year','CanonicalCategory','Film','FilmId','Name','Winner'],    
    dtype_backend='pyarrow'
    )
    .rename(columns={'CanonicalCategory': 'Category'})
    .replace({'Winner':{pd.NA: False}})
)
cleaning ish
 astype([VALUE])
.fillna(0)
.find values of NaN to convert? - use value counts or somehting better (see pandas 062
Different data sources
.str.title()
.str.strip()

#Summarise
- df.shape
- df.describe()
- df.corr()
  - isna.sum()
- dtypes

- memory
  df.memory_usage(deep=True).sum()

Summarising
- value_counts
- count
- 
instead of sort_values().head() use nlargest()/nsmallest() - can combine with agg


temp columns
.assign

Indexes
df.set_index("col")
df.reset_index()
An index is a list, so when changing can use list comprehension to loop over
Date index:
- resample = groupby on timeseries, i.e. will work on sequential dates, so grouping on month will preserve the year
- slicing, df.loc['2025-01':] = Jan 1st 2025 onwards, df.loc[DATE:DATE] = between dates
Multi Indexes
- use xs to slice data
- use unstack to compare two columns from same level
- create: number of methods on MultiIndex - .from_tuples, .from_arrays, .from_product
from_product takes two iterables and produces the product of them in cartesian space

- index.intersection
Filtering
loc - df.loc[row filters, columns to keep]
if need to filter twice with loc, then second  needs to be a lambda to stop the returning of results from the initial dataframe e.g.
    df
    .loc['2023']
    .loc[lambda df_: df_['Demographic'] == 'UC / Single Minors']
  regex .loc[lambda df_: df_['Category'].str.contains(r'[REGEX]', regex=True)]

multiple text = .loc[lambda df_: df_['Question'].isin(['Number of retrievals', 
            'Number of transfers'])]

Can pass an array into loc
array = ['a','b']
df.loc[array]
JSON
- if dictionaires in columns
- get values = str.get(ELEMENT) which is equal to df[column][ELEMENT]

Basic

Advanced

Grouping

Aggregate functions
- multiple, i.e. [min, max]
- use idxmin, idxmax to get the index location
- apply a lambda to df that has a groupby to filter grouped rows
	 e.g.
	df.groupby(COL_1)[COL_2].nunique.loc[lambda s_: s_ > 1] this returns only values of COL_1 that have a value greater than 1

- stack / unstack
- unstack multi indexes, level determines which index goes to columns, so can avoid .T

Date indexes

String formatting

format numbers as strings:

.apply(lambda x: f'{x:,.2f}')

The “,” tells Python to put commas every 3 digits
The “.” tells Python that we want to specify the number of digits after the decimal point, in this case 2 digits
The “f” tells Python that we’re dealing with floating-point values

Plotting

If you invoke “plot.line” on a Pandas data frame, then you’ll get one line per column, with a shared x axis corresponding to the data frame’s index
