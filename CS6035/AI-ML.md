# pandas  
https://madewithml.com/courses/mlops/exploratory-data-analysis/  
https://madewithml.com/courses/foundations/pandas/  

DataFrames - CSV, Excel, SQL  
Handles missing values - ```dropna(), fillna()```  
Data Exploration - ```describe(), groupby()```  
Vizualization - ```pivot_table(), melt()```  
Merging Datasets - ```merge(), concat()```  
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("<File>", sep="\t", delimiter=",")
print(df.head())

# Summary Information
df.describe()

# Histograms
df["<Column>"].hist()

# Unique Values
df["<Column>"].unique()

# Filtering Values
df[df["<Column>"]=="<Value>"].head()

# Sorting
df.sort_values("<Column>", ascending=False).head()

# Grouping
group = df.groupby("<Column>")
group.mean()

# Indexing
df.iloc[0, :]      # Row 0
df.iloc[0, 1]      # Row 0, Column 1


```

# scikit-learn
```
```

# TensorFlow
```
```

# PyTorch  
```
```
