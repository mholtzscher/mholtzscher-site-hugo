---
title: "What Is The Most Popular GitHub License?"
date: 2018-03-07T20:00:00-05:00
---

The data for this project is taken from Google's public BigQuery datasets. The dataset can be easily accessed on Kaggle via the [GitHub Repos](https://www.kaggle.com/github/github-repos) dataset.

### Getting The Data

```python
import pandas as pd
from google.cloud import bigquery
from bq_helper import BigQueryHelper
```


```python
bq = BigQueryHelper('bigquery-public-data', 'github_repos')
```


```python
QUERY = """
        SELECT license, COUNT(license) as license_count
        FROM `bigquery-public-data.github_repos.licenses`
        GROUP BY license
        """

df = bq.query_to_pandas_safe(QUERY)
```


```python
ax = df.sort_values('license_count').plot.barh(x='license', y='license_count', legend=False)
ax.set_xlabel('License')
ax.set_ylabel('Count')
ax.set_title('Distribution of GitHub Licenses')
```




![png](/github/licenses.png)

