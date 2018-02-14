---
title: "2016 US Presedential Election Contributions - Donations (Part 3)"
date: 2017-08-07T15:00:00-05:00
---

In this post I wanted to look at the distribution of donation sizes. I was curious if one of the candidates was better at getting small donations versus large contributions from organizations.

Data is from the [FEC](http://classic.fec.gov/disclosurep/PDownload.do)


```python
import pandas as pd
import plotly.plotly as py
import cufflinks as cf
```


```python
cf.set_config_file(theme='polar')
cf.go_online()
```

### Preparing The Data


```python
df = pd.read_csv("P00000001-ALL.csv", index_col=False)
```


#### Filtering Out Refunds


```python
df = df[df.contb_receipt_amt > 0]
```

#### Create A DataFrame Filtered To The Final Major Candidates


```python
df = df[df.cand_nm.isin(['Trump, Donald J.','Clinton, Hillary Rodham'])]
```

### What Is The Most Common Contribution Size?


```python
bins = [0, 5, 10, 25, 50, 75, 100, 500, 1000, 10000, 100000, 1000000, 10000000, 100000000]
group_names = ['0 to 5', '5 to 10', '10 to 25', '25 to 50', '50 to 75', '75 to 100', '100 to 500', 
                '500 to 1000', '1000 to 10000', '10000 to 100000', '100000 to 1000000', 
               '1000000 to 10000000', '10000000 to 100000000']

categories = pd.cut(df['contb_receipt_amt'], bins, labels=group_names)
df['categories'] = pd.cut(df['contb_receipt_amt'], bins, labels=group_names)
df['cat_str'] = df['categories'].astype('str')
```


```python
df['cat_str'].value_counts().iplot(kind='bar', yTitle='Number of Contributions',
                                   title='Distribution of Contribution Sizes', filename='2016-fec-donation-sizes')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/101.embed" height="525px" width="100%"></iframe>



### What Is The Most Common Contribution Size By Candidate? 


```python
r = df[['cat_str', 'cand_nm']].groupby('cat_str').cand_nm.value_counts()
r = r.unstack().reset_index().set_index('cat_str')
r = r.reindex(index = group_names)
r.iplot(kind='bar', barmode='stack', filename='2016-fec-contribution-size-comparison', 
        yTitle='Number of Contributions', xTitle='Contribution Range In USD')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/103.embed" height="525px" width="100%"></iframe>



---
The Jupyter Notebook for this work can be found on [GitHub](https://github.com/mholtzscher/2016_fec_presidential/blob/master/2016_presidential_contributions-bins.ipynb).
