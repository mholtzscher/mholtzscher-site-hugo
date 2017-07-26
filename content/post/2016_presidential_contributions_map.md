---
title: "2016 US Presedential Election Contributions - Maps (Part 2)"
date: 2017-07-26T15:00:00-05:00
---

Continuing from my last post, I wanted to see how the distribution of donations looked across the US states.

Data is from the [FEC](http://classic.fec.gov/disclosurep/PDownload.do)


```python
import pandas as pd
import plotly.plotly as py
import plotly.offline as offline
import cufflinks as cf
```

### Preparing The Data


```python
url = 'contributors.csv'
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

#### Split By Party


```python
democrats = df[df['party'] == 'Democrat']
democrats = democrats.groupby('contbr_st').contb_receipt_amt.sum()
democrats = democrats.to_frame().reset_index()

republicans = df[df['party'] == 'Republican']
republicans = republicans.groupby('contbr_st').contb_receipt_amt.sum()
republicans = republicans.to_frame().reset_index()
```

#### Filter To Valid States


```python
states = ['AL','AK','AZ','AR','CA','CO','CT','DE','FL','GA','HI','ID','IL','IN','IA','KS',
          'KY','LA','ME','MD','MA','MI','MN','MS','MO','MT','NE','NV','NH','NJ','NM','NY',
          'NC','ND','OH','OK','OR','PA','RI','SC','SD','TN','TX','UT','VT','VA','WA','WV','WI','WY']
democrats = democrats[democrats.contbr_st.isin(states)]
republicans = republicans[republicans.contbr_st.isin(states)]
```

## Contribution Map For Democrats


```python
scl = [[0.0, 'rgb(239,243,255)'],[0.2, 'rgb(198,219,239)'],[0.4, 'rgb(158,202,225)'],\
            [0.6, 'rgb(107,174,214)'],[0.8, 'rgb(49,130,189)'],[1.0, 'rgb(8,81,156)']]

data = [ dict(
        type='choropleth',
        colorscale = scl,
        autocolorscale = False,
        locations = democrats['contbr_st'],
        z = democrats['contb_receipt_amt'].astype(float),
        locationmode = 'USA-states',
        marker = dict(
            line = dict (
                color = 'rgb(255,255,255)',
                width = 2
            ) ),
        colorbar = dict(
            title = "Millions USD")
        ) ]

layout = dict(
        title = '2016 US Election Contributions(Democrats)',
        geo = dict(
            scope='usa',
            projection=dict( type='albers usa' ),
            showlakes = True,
            lakecolor = 'rgb(255, 255, 255)'),
             )
    
fig = dict( data=data, layout=layout )
py.iplot( fig, filename='2016-election-democrat-ammount-map' )
```


<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/99.embed" height="525px" width="100%"></iframe>


## Contribution Map For Republicans


```python
scl = [[0.0, 'rgb(254,229,217)'],[0.2, 'rgb(252,187,161)'],[0.4, 'rgb(252,146,114)'],\
            [0.6, 'rgb(251,106,74)'],[0.8, 'rgb(222,45,38)'],[1.0, 'rgb(165,15,21)']]

data = [ dict(
        type='choropleth',
        colorscale = scl,
        autocolorscale = False,
        locations = republicans['contbr_st'],
        z = republicans['contb_receipt_amt'].astype(float),
        locationmode = 'USA-states',
        marker = dict(
            line = dict (
                color = 'rgb(255,255,255)',
                width = 2
            ) ),
        colorbar = dict(
            title = "Millions USD")
        ) ]

layout = dict(
        title = '2016 US Election Contributions(Republican)',
        geo = dict(
            scope='usa',
            projection=dict( type='albers usa' ),
            showlakes = True,
            lakecolor = 'rgb(255, 255, 255)'),
             )
    
fig = dict( data=data, layout=layout )
py.iplot( fig, filename='2016-election-republican-map' )
```


<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~mholtzscher/97.embed" height="525px" width="100%"></iframe>


---
The Jupyter Notebook for this work can be found on [GitHub](https://github.com/mholtzscher/2016_fec_presidential/blob/master/2016_presidential_contributions-map.ipynb).
