---
title: Creating Healthy Communities Coalition (CHCC) Activities 
subtitle: An Ohio Department of Health (ODH) program 
summary: Estimated number of people impacted by CHCC activities implemented in 2015-2017 
authors:
- andre
tags: [Jupyter, Health Promotion]
categories: [Python, Public Health]
date: "2020-02-03T00:00:00Z"
lastMod: "2020-02-03T00:00:00Z"
featured: true
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: ""
  focal_point: ""

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

```python
import sys
print("Python version")
print (sys.version)
print("Version info.")
print (sys.version_info)

```

    Python version
    3.7.4 (default, Aug 13 2019, 20:35:49) 
    [GCC 7.3.0]
    Version info.
    sys.version_info(major=3, minor=7, micro=4, releaselevel='final', serial=0)



```python
# make sure to install these packages before running:
# pip install pandas
# pip install sodapy

import pandas as pd
from sodapy import Socrata

# Unauthenticated client only works with public data sets. Note 'None'
# in place of application token, and no username or password:
client = Socrata("data.cincinnati-oh.gov", None)

# Example authenticated client (needed for non-public datasets):
# client = Socrata(data.cincinnati-oh.gov,
#                  MyAppToken,
#                  userame="user@example.com",
#                  password="AFakePassword")

# First 2000 results, returned as JSON from API / converted to Python list of
# dictionaries by sodapy.
chcc_activities_soda = client.get("skqm-k58y", limit=2000)

# Convert to pandas DataFrame
chcc_activities = pd.DataFrame.from_records(chcc_activities_soda)
```

    WARNING:root:Requests made without an app_token will be subject to strict throttling limits.



```python
print("Summary of the basic information about this DataFrame and its data:")
print(chcc_activities.info())
```

    Summary of the basic information about this DataFrame and its data:
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 406 entries, 0 to 405
    Data columns (total 17 columns):
     #   Column               Non-Null Count  Dtype 
    ---  ------               --------------  ----- 
     0   category             406 non-null    object
     1   topics               406 non-null    object
     2   intervention         406 non-null    object
     3   description          406 non-null    object
     4   status               406 non-null    object
     5   outcome              406 non-null    object
     6   implementation_date  406 non-null    object
     7   location             406 non-null    object
     8   address              406 non-null    object
     9   zip_code             406 non-null    object
     10  zipcode_pop_2016     406 non-null    object
     11  partners             406 non-null    object
     12  longitude            406 non-null    object
     13  latitude             406 non-null    object
     14  neighborhood         406 non-null    object
     15  zipcode_pop_2010     406 non-null    object
     16  notes                8 non-null      object
    dtypes: object(17)
    memory usage: 54.0+ KB
    None



```python
chcc_activities.describe(include='all')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>category</th>
      <th>topics</th>
      <th>intervention</th>
      <th>description</th>
      <th>status</th>
      <th>outcome</th>
      <th>implementation_date</th>
      <th>location</th>
      <th>address</th>
      <th>zip_code</th>
      <th>zipcode_pop_2016</th>
      <th>partners</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>neighborhood</th>
      <th>zipcode_pop_2010</th>
      <th>notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>8</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>3</td>
      <td>8</td>
      <td>3</td>
      <td>9</td>
      <td>1</td>
      <td>8</td>
      <td>11</td>
      <td>345</td>
      <td>386</td>
      <td>123</td>
      <td>124</td>
      <td>10</td>
      <td>378</td>
      <td>378</td>
      <td>47</td>
      <td>123</td>
      <td>8</td>
    </tr>
    <tr>
      <th>top</th>
      <td>HEALTHY EATING</td>
      <td>ENVIRONMENTAL HEALTH</td>
      <td>SYSTEMS</td>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>COMPLETED</td>
      <td>CRIME PREVENTION THROUGH ENVIRONMENTAL DESIGN</td>
      <td>2016-04-01T00:00:00.000</td>
      <td>COLUMBUS (UDF LOCATION)</td>
      <td>3951 W 8TH ST</td>
      <td>45202</td>
      <td>13761</td>
      <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
      <td>-84.5120196</td>
      <td>39.1031182000299</td>
      <td>OUTSIDE CITY LIMITS</td>
      <td>15483</td>
      <td>ADDRESS WOULD NOT GEOCODE, USED NEIGHBORING BU...</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>198</td>
      <td>174</td>
      <td>210</td>
      <td>173</td>
      <td>406</td>
      <td>174</td>
      <td>173</td>
      <td>23</td>
      <td>4</td>
      <td>19</td>
      <td>19</td>
      <td>173</td>
      <td>8</td>
      <td>8</td>
      <td>156</td>
      <td>19</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
import sqlalchemy
import psycopg2
engine = sqlalchemy.create_engine('postgresql://dre:password@localhost:5432/cincinnati')
chcc_activities.to_sql('chcc_activities', engine)
```


```python
connection = psycopg2.connect(user="dre",
                                  password="password",
                                  host="localhost",
                                  port="5432",
                                  database="cincinnati")
cursor = connection.cursor()
postgreSQL_select_Query = "SELECT * FROM chcc_activities"

cursor.execute(postgreSQL_select_Query)
print("Selecting rows from chcc_activities table using cursor.fetchall")
chcc_activities_sql = cursor.fetchall()

if(connection):
      cursor.close()
      connection.close()
      print("PostgreSQL connection is closed")
```

    Selecting rows from chcc_activities table using cursor.fetchall
    PostgreSQL connection is closed



```python
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql



```python
%sql postgresql://dre:password@localhost:5432/cincinnati
```




    'Connected: dre@cincinnati'




```python
%%sql 
SELECT *
FROM chcc_activities
WHERE category =  'HEALTHY EATING'
ORDER BY index
FETCH FIRST 5 ROWS ONLY
```

     * postgresql://dre:***@localhost:5432/cincinnati
    5 rows affected.





<table>
    <tr>
        <th>index</th>
        <th>category</th>
        <th>topics</th>
        <th>intervention</th>
        <th>description</th>
        <th>status</th>
        <th>outcome</th>
        <th>implementation_date</th>
        <th>location</th>
        <th>address</th>
        <th>zip_code</th>
        <th>zipcode_pop_2016</th>
        <th>partners</th>
        <th>longitude</th>
        <th>latitude</th>
        <th>neighborhood</th>
        <th>zipcode_pop_2010</th>
        <th>notes</th>
    </tr>
    <tr>
        <td>3</td>
        <td>HEALTHY EATING</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>SYSTEMS</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>COMPLETED</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>SHARONVILLE (UDF LOCATION)</td>
        <td>12191 MOSTELLER RD</td>
        <td>45241</td>
        <td>23707</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>-84.4287401442128</td>
        <td>39.2982939357563</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>23044</td>
        <td>None</td>
    </tr>
    <tr>
        <td>4</td>
        <td>HEALTHY EATING</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>SYSTEMS</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>COMPLETED</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>WASHINGTON COURT HOUSE (UDF LOCATION)</td>
        <td>1710 COLUMBUS AV</td>
        <td>43160</td>
        <td>22171</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>-84.3418176</td>
        <td>39.0769917000299</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>22305</td>
        <td>None</td>
    </tr>
    <tr>
        <td>9</td>
        <td>HEALTHY EATING</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>SYSTEMS</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>COMPLETED</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>WEST UNION (UDF LOCATION)</td>
        <td>735 E MAIN ST</td>
        <td>45693</td>
        <td>8305</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>-82.9797113</td>
        <td>39.9576447000294</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>8981</td>
        <td>None</td>
    </tr>
    <tr>
        <td>11</td>
        <td>HEALTHY EATING</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>SYSTEMS</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>COMPLETED</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>WILDER (UDF LOCATION)</td>
        <td>509 LICKING PIKE</td>
        <td>41071</td>
        <td>20724</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>-84.4866546</td>
        <td>39.0553626000299</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>20869</td>
        <td>None</td>
    </tr>
    <tr>
        <td>12</td>
        <td>HEALTHY EATING</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>SYSTEMS</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>COMPLETED</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>HUBER HEIGHTS (UDF LOCATION)</td>
        <td>7224 TAYLORSVILLE RD</td>
        <td>45424</td>
        <td>50670</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>-84.105625</td>
        <td>39.8605103000295</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>49930</td>
        <td>None</td>
    </tr>
</table>




```python
udf_intervention = %sql SELECT description FROM chcc_activities WHERE partners =  'UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH' ORDER BY description FETCH FIRST 5 ROWS ONLY
    
print(udf_intervention)
```

     * postgresql://dre:***@localhost:5432/cincinnati
    5 rows affected.
    +---------------------------------------------------------------------------+
    |                                description                                |
    +---------------------------------------------------------------------------+
    | HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO GO" CUPS AND SALADS) AT UDF |
    | HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO GO" CUPS AND SALADS) AT UDF |
    | HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO GO" CUPS AND SALADS) AT UDF |
    | HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO GO" CUPS AND SALADS) AT UDF |
    | HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO GO" CUPS AND SALADS) AT UDF |
    +---------------------------------------------------------------------------+



```python
udf_intervention = %sql SELECT description, intervention FROM chcc_activities WHERE partners =  'UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH' ORDER BY description
udf_intervention_df = udf_intervention.DataFrame()
udf_intervention_df

```

     * postgresql://dre:***@localhost:5432/cincinnati
    173 rows affected.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>description</th>
      <th>intervention</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>1</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>2</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>3</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>168</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>169</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>170</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>171</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>172</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
  </tbody>
</table>
<p>173 rows × 2 columns</p>
</div>




```python
type(udf_intervention_df)
```




    pandas.core.frame.DataFrame




```python
import matplotlib.pyplot as plt
plt.figure(figsize=(18,7))
chart = %sql SELECT category, count(category) FROM chcc_activities GROUP BY category
chart.bar()
```

     * postgresql://dre:***@localhost:5432/cincinnati
    3 rows affected.





    <BarContainer object of 3 artists>




![png](./index_13_2.png)



```python
import matplotlib.pyplot as plt
plt.figure(figsize=(18,7))
chart = %sql SELECT topics, count(topics) FROM chcc_activities GROUP BY topics
chart.bar()
```

     * postgresql://dre:***@localhost:5432/cincinnati
    8 rows affected.





    <BarContainer object of 8 artists>




![png](./index_14_2.png)


# References

1. [Creating Healthy Communities Coalition (CHCC) Activities](https://dev.socrata.com/foundry/data.cincinnati-oh.gov/skqm-k58y)
1. [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/index.html)
1. [SQLAlchemy — Python Tutorial](https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91)
1. [How to write DataFrame to postgres table?](https://stackoverflow.com/questions/23103962/how-to-write-dataframe-to-postgres-table)
1. [Python: Get the Python version](https://www.w3resource.com/python-exercises/python-basic-exercise-2.php)
1. [Python Select from PostgreSQL Table](https://pynative.com/python-postgresql-select-data-from-table/)
1. [Here’s How to Run SQL in Jupyter Notebooks](https://towardsdatascience.com/heres-how-to-run-sql-in-jupyter-notebooks-f26eb90f3259)
