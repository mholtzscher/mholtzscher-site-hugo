---
title: "2016 US Presedential Election Contributions (Part 1)"
date: 2017-07-21T08:00:00-05:00
---

I recently came across this dataset from the Federal Election Commission and wanted to explore it a bit to see how money affected the election. Each plot aims to try and answer a question that I thought of while looking through the available data. This post is part 1 of my exploration and visualization of this dataset. 

Data is from the [FEC](http://classic.fec.gov/disclosurep/PDownload.do)


```python
import pandas as pd
import numpy as np
import plotly.plotly as py
import plotly.figure_factory as ff
import plotly.graph_objs as go
import plotly.offline as offline
import cufflinks as cf
```


```python
cf.set_config_file(theme='polar')
cf.go_online()
```

### Preparing The Data


```python
url = 'https://storage.googleapis.com/mholtzscher-datasets/2016%20FEC%20Presidential/contributors.csv'
df = pd.read_csv(url, index_col=False)
```  
    

#### Matching Candidates To Their Parties


```python
parties = {'Rubio, Marco': 'Republican',
           'Santorum, Richard J.': 'Republican',
           'Perry, James R. (Rick)': 'Republican',
           'Carson, Benjamin S.': 'Republican',
           "Cruz, Rafael Edward 'Ted'": 'Republican',
           'Paul, Rand': 'Republican',
           'Clinton, Hillary Rodham': 'Democrat',
           'Sanders, Bernard': 'Democrat',
           'Fiorina, Carly': 'Republican',
           'Huckabee, Mike': 'Republican',
           'Pataki, George E: .': 'Republican',
           "O'Malley, Martin Joseph": 'Democrat',
           'Graham, Lindsey O.': 'Republican',
           'Bush, Jeb': 'Republican',
           'Trump, Donald J.': 'Republican',
           'Jindal, Bobby': 'Republican',
           'Christie, Christopher J.': 'Republican',
           'Walker, Scott': 'Republican',
           'Stein, Jill': "Green Party",
           'Webb, James Henry Jr.': 'Democrat',
           'Kasich, John R.': 'Republican',
           'Gilmore, James S III': 'Republican',
           'Lessig, Lawrence': 'Democrat',
           'Johnson, Gary': 'Libertarian',
           'McMullin, Evan': 'Independent'}
df['party'] = df.cand_nm.map(parties)
```

#### Filtering Out Refunds


```python
df = df[df.contb_receipt_amt > 0]
```

#### Cleaning Up Occupations


```python
occ_mapping = {
    'INFORMATION REQUESTED PER BEST EFFORTS' : 'NOT PROVIDED',
    'INFORMATION REQUESTED' : 'NOT PROVIDED',
    'INFORMATION REQUESTED (BEST EFFORTS)' : 'NOT PROVIDED',
    'C.E.O.': 'CEO'
}

f = lambda x: occ_mapping.get(x, x)
df.contbr_occupation = df.contbr_occupation.map(f)
```

#### Create A DataFrame Filtered To The Final Major Candidates


```python
dff = df[df.cand_nm.isin(['Trump, Donald J.','Clinton, Hillary Rodham'])]
```


### Which Party Received The Most Contributions?


```python
party = df.groupby('party').contb_receipt_amt.sum().to_frame().reset_index()
party.iplot(kind='pie', textinfo='value+percent', textposition='outside', labels='party', 
            values='contb_receipt_amt', filename='fec-2016-party-contributions',
           title='2016 Total Contributions(USD) By Party')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/77.embed" height="525px" width="100%"></iframe>



### Which Candidate Recieved The Most In Contributions?


```python
candidate = df.groupby('cand_nm').contb_receipt_amt.sum().sort_values(ascending=False)
candidate.iplot(kind='bar', yTitle='Contributions(USD)', 
    title='2016 Contributions(USD) By Presidential Candidate', filename='fec-2016-candidate-contributions')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/79.embed" height="525px" width="100%"></iframe>



### Which Occupations Donated The Most Number Of Times?


```python
job = df['contbr_occupation'].value_counts()
job[:25].iplot(kind='bar', filename='fec-2016-top-occupation-volume', yTitle='Number of Contributions', 
               title="Top 25 Occupations By Number of Contributions")
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/81.embed" height="525px" width="100%"></iframe>



### Which Occupations Donated The Most?


```python
job = df.groupby('contbr_occupation').contb_receipt_amt.sum().sort_values(ascending=False)
job[:25].iplot(kind='bar', filename='fec-2016-top-occupation-amounts', yTitle='Total Contribution', 
               title="Top 25 Occupations By Contribution Ammount")
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/83.embed" height="525px" width="100%"></iframe>



### Which Occupations Donated The Most to Hillary Clinton?


```python
job = df[df['cand_nm'] == 'Clinton, Hillary Rodham'].groupby('contbr_occupation').contb_receipt_amt.sum().sort_values(ascending=False)
job[:25].iplot(kind='bar', filename='fec-2016-top-occupation-amounts-clinton', yTitle='Total Contribution', 
               title="Clinton Donors Top 25 Occupations By Contribution Ammount")
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/85.embed" height="525px" width="100%"></iframe>



### Which Occupations Donated The Most to Donald Trump?


```python
job = df[df['cand_nm'] == 'Trump, Donald J.'].groupby('contbr_occupation').contb_receipt_amt.sum().sort_values(ascending=False)
job[:25].iplot(kind='bar', filename='fec-2016-top-occupation-amounts-trump', yTitle='Total Contribution', 
               title="Trump Donors Top 25 Occupations By Contribution Ammount")
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/87.embed" height="525px" width="100%"></iframe>



### Which State Donated The Most Number Of Times?


```python
state = dff.contbr_st.value_counts().sort_values(ascending=False)[:20]
state.iplot(kind='bar', filename='fec-2016-state-contributions-volume')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/89.embed" height="525px" width="100%"></iframe>



### Which State Donated The Most?


```python
state_ammt = dff.groupby('contbr_st').contb_receipt_amt.sum().sort_values(ascending=False)[:20]
state_ammt.iplot(kind='bar', filename='fec-2016-state-contributions-ammount')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/91.embed" height="525px" width="100%"></iframe>



---
The Jupyter Notebook for this work can be found on [GitHub](https://github.com/mholtzscher/2016_fec_presidential/blob/master/2016_presidential_contributions.ipynb).
