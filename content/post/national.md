---
title: "US National Baby Names"
date: 2017-07-16T15:00:00-05:00
---

The data for this project is taken from Kaggle [US Baby Names](https://www.kaggle.com/kaggle/us-baby-names) dataset. It consists of Social Security Administration baby naming data from 1880 to 2014. Note that only names with at least 5 babies born in the same year (/ state) are included in this dataset for privacy.

### Preparing The Data


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
font_size = 16
```


```python
df = pd.read_csv('NationalNames.csv')
df.head()
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Name</th>
      <th>Year</th>
      <th>Gender</th>
      <th>Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Mary</td>
      <td>1880</td>
      <td>F</td>
      <td>7065</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Anna</td>
      <td>1880</td>
      <td>F</td>
      <td>2604</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Emma</td>
      <td>1880</td>
      <td>F</td>
      <td>2003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Elizabeth</td>
      <td>1880</td>
      <td>F</td>
      <td>1939</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Minnie</td>
      <td>1880</td>
      <td>F</td>
      <td>1746</td>
    </tr>
  </tbody>
</table>
</div>



### Births Per Year

Note: This plot is not a completely accurate representation of the number of births in the US since our data only accounts for names where at least 5 babies were given that name. This chart is to just get a basic understanding of the number of births.


```python
total_per_year = df.groupby('Year').sum()
axis = total_per_year['Count'].plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Births Per Year(Estimated)', fontsize=30)
```

![png](/national_files/national_5_1.png)


### Births Per Year By Gender

Note: This plot is not a completely accurate representation of the number of births in the US since our data only accounts for names where at least 5 babies were given that name. This chart is to just get a basic understanding of the number of births and differences by gender.


```python
gender = df.groupby(['Year', 'Gender']).sum()
axis = gender['Count'].unstack().plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Births Per Year By Gender', fontsize=30)
axis.legend(fontsize=font_size)
```

![png](/national_files/national_7_1.png)


### Proportion of Male to Female Births Per Year

I was curious to see the difference in numbers between male and female births. I expected to be near 50/50 but this was not the case. This sparked some interesting questions of "Why are there more males born in the last 70 years?" and "Why the flip around the 1940s?" Perhaps this will be a future exploration.


```python
ratio = df.groupby(['Year', 'Gender']).sum()
ratio = ratio.unstack()
ratio['ratio'] = ratio['Count'].apply(lambda row: row['M'] / row['F'], axis=1)
axis = ratio['ratio'].plot.line(figsize=(20,10), grid=True, fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Proportion(Male/Female)', fontsize=30)
axis.set_title('Proportion of Male to Female Births Per Year', fontsize=30)
```

![png](/national_files/national_9_1.png)


### Top 10 Names All Time


```python
top_names = df.groupby('Name').sum()
top_names = top_names['Count'].sort_values(ascending=False)[:10]
top_10 = []
for i in top_names.iteritems():
    top_10.append(i[0])

top_names = df[df['Name'].isin(top_10)].groupby(['Year', 'Name']).sum()
axis = top_names['Count'].unstack().plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Top 10 Names (1880-2014)', fontsize=30)
axis.legend(fontsize=font_size)
```

![png](/national_files/national_11_1.png)


### Top 10 Female Names All Time


```python
top_names = df[df['Gender'] == 'F'].groupby('Name').sum()
top_names = top_names['Count'].sort_values(ascending=False)[:10]
top_10 = []
for i in top_names.iteritems():
    top_10.append(i[0])

top_names = df[df['Name'].isin(top_10)].groupby(['Year', 'Name']).sum()
axis = top_names['Count'].unstack().plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Top 10 Female Names (1880-2014)', fontsize=30)
axis.legend(fontsize=font_size)
```

![png](/national_files/national_13_1.png)


### Top 10 Male Names All Time


```python
top_names = df[df['Gender'] == 'M'].groupby('Name').sum()
top_names = top_names['Count'].sort_values(ascending=False)[:10]
top_10 = []
for i in top_names.iteritems():
    top_10.append(i[0])

top_names = df[df['Name'].isin(top_10)].groupby(['Year', 'Name']).sum()
axis = top_names['Count'].unstack().plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Top 10 Male Names (1880-2014)', fontsize=30)
axis.legend(fontsize=font_size)
```

![png](/national_files/national_15_1.png)


### Diversity In Names
Upon looking at the top 10 names I noticed how they dropped off as they approached today. This was interesting considering that births per year have trended up consistently. This plot looks at the total number of names used each year.


```python
import math
def f(x):
    if not math.isnan(x):
        return 1

diversity = df.groupby(['Name', 'Year']).sum()
diversity = diversity.unstack().applymap(f)
diversity = diversity.sum(axis=0)
axis = diversity['Count'].plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Number of Names Used', fontsize=30)
axis.set_title('Diversity In Names', fontsize=30)
```

![png](/national_files/national_17_1.png)


### Most Consistent Names

After seeing the variety of names great increase I was curious if there were any names that were consistently used. This plot looks at the 10 names with the lowest standard deviation over the full time range.


```python
consistent = df.groupby(['Name', 'Year']).sum()
consistent = consistent.unstack().std(axis=1, skipna=False).sort_values().dropna()[:10]

top_10 = list(consistent.index.values)

con_names = df[df['Name'].isin(top_10)].groupby(['Year', 'Name']).sum()
axis = con_names['Count'].unstack().plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Most Consistent Names', fontsize=30)
axis.legend(fontsize=font_size)
```

![png](/national_files/national_19_1.png)


### Least Consistent Names

This plot looks at the 10 names with the greatest standard deviation over the full time range.


```python
consistent = df.groupby(['Name', 'Year']).sum()
consistent = consistent.unstack().std(axis=1, skipna=False).sort_values().dropna()[-10:]

top_10 = list(consistent.index.values)

con_names = df[df['Name'].isin(top_10)].groupby(['Year', 'Name']).sum()
axis = con_names['Count'].unstack().plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Least Consistent Names', fontsize=30)
axis.legend(fontsize=font_size)
```

![png](/national_files/national_21_1.png)


### All The Michaels

Just for fun what is the history of my name.


```python
michael = df[df['Name'] == 'Michael'].groupby('Year').sum()
axis = michael['Count'].plot.line(figsize=(20,10), fontsize=font_size)
axis.set_xlabel('Year', fontsize=30)
axis.set_ylabel('Births', fontsize=30)
axis.set_title('Babies Named Michael By Year', fontsize=30)
```

![png](/national_files/national_23_1.png)


---

The Jupyter Notebook for this work can be found on [GitHub](https://github.com/mholtzscher/eda-notebooks/blob/master/baby-names/national.ipynb).
