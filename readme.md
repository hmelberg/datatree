# DataTree

## All your data easily accessible in one place

## The idea
DataTree it is a way of having many dataframes, or other types of data in one object, that can be accessed using dot notation

## Example
When analyzing different health data it is common to have many dataframes or dictionaries that contain the labels for the codes that are used to describe diagnoses, pharmaceuticals and procedures. It might also be useful to have some metadata about each dataframe; for instance source of the codes (url). The idea to have one object, here labelled codes, with all the other data as sub-frames in a tree structure:

```python
# a dataframe of all icd codes and related text.
codes.icd.df 

# metainformation about the icd dataframe
codes.icd.description 

# a dataframe with the heart codes within the icd codes
codes.icd.heart.df 

# a dataframe of the codes related to cancer pharmaceuticals in the atc dataframe
codes.atc.cancer.df 

# a dataframe of the icd codes and their Spanish text equivalents
codes.icd.spanish 
```

## How?
```python
# constructe a DataTree object
codes = DataTree()

# insert a dataframe at a branch in the tree, with metadata (information about version of the icd data)
codes.insert(name='icd', df=icd, version=11)

# access the dataframe (all standard dataframe methods work)
codes.icd.df
codes.icd.df.text.str.contains('heart')

# insert a list of only heart codes
codes.icd.insert(name='heart', data=['C50', 'C51'])

# use it
df[df.icd.isin(codes.icd.heart.data)]
```

## History
The starting point was a need to maintain several dataframes with different codebooks useful when analyzing health data: events in hospital have codes for different diagnoses, a prescription use codes as shorthand for a longer text that describe. I also often needed to refer to only a subset of one code system, for instance only the codes related to heart disease out of all possible diagnostic codes. There are ways of doing this, using dctionaries, separate dataframes and so on, but it became messy or verbose. Nested dictionaries are cumbersome to work with. Organizing everything in a large dataframe and using hierarchical indexing is another potential solution, but it was not a good solution if the dataframes were heterogenous. Moreover, dataframe do not have metadata, and I frequently needed metadata to be associatted with the dataframes. For instance: Which version of the code system did the dataframe contain? Where was the data obtained (url)? When was it last updated? What language? Lastly, the tree did not necesarily have to only contain dataframes: It should be flexible enough to contain all types of data!

DataTree is an experiment based on this experience, but the idea generalized: It allows you to construct a tree of different data objects, dataframes, lists, series, dictionaries. Each of these can be referred to using dot notatin along the branches in the tree. Each object also maintains its usual methods. Medical codebooks is just one example of this.

## Alternatives

Nested dictionaries could be used to acive the same, but codes['icd']['cancer']['spanish'] is tiresome to write if you use it often.

One solution could be Box which has nested dictionaries with dot notations.




