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
* <span style="color: #fd7f6f;">Healthy Eathing</span>

* <span style="color: #7eb0d5;">Active Living</span>

* <span style="color: #b2e061;">Tobacco Free Living</span>

{{% toc %}}


```python
import sys
print("Python version")
print (sys.version)
print("Version info.")
print (sys.version_info)

```

    Python version
    3.7.1 (v3.7.1:260ec2c36a, Oct 20 2018, 14:57:15) [MSC v.1915 64 bit (AMD64)]
    Version info.
    sys.version_info(major=3, minor=7, micro=1, releaselevel='final', serial=0)
    


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
    address                406 non-null object
    category               406 non-null object
    description            406 non-null object
    implementation_date    406 non-null object
    intervention           406 non-null object
    latitude               406 non-null object
    location               406 non-null object
    longitude              406 non-null object
    neighborhood           406 non-null object
    notes                  8 non-null object
    outcome                406 non-null object
    partners               406 non-null object
    status                 406 non-null object
    topics                 406 non-null object
    zip_code               406 non-null object
    zipcode_pop_2010       406 non-null object
    zipcode_pop_2016       406 non-null object
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
      <th>address</th>
      <th>category</th>
      <th>description</th>
      <th>implementation_date</th>
      <th>intervention</th>
      <th>latitude</th>
      <th>location</th>
      <th>longitude</th>
      <th>neighborhood</th>
      <th>notes</th>
      <th>outcome</th>
      <th>partners</th>
      <th>status</th>
      <th>topics</th>
      <th>zip_code</th>
      <th>zipcode_pop_2010</th>
      <th>zipcode_pop_2016</th>
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
      <td>8</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
      <td>406</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>386</td>
      <td>3</td>
      <td>9</td>
      <td>11</td>
      <td>3</td>
      <td>378</td>
      <td>345</td>
      <td>378</td>
      <td>47</td>
      <td>8</td>
      <td>8</td>
      <td>10</td>
      <td>1</td>
      <td>8</td>
      <td>123</td>
      <td>123</td>
      <td>124</td>
    </tr>
    <tr>
      <th>top</th>
      <td>3951 W 8TH ST</td>
      <td>HEALTHY EATING</td>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>2016-04-01T00:00:00.000</td>
      <td>SYSTEMS</td>
      <td>39.1031182000299</td>
      <td>COLUMBUS (UDF LOCATION)</td>
      <td>-84.5120196</td>
      <td>OUTSIDE CITY LIMITS</td>
      <td>ADDRESS AN INTERSECTION, USED FOLLOWING ADDRES...</td>
      <td>CRIME PREVENTION THROUGH ENVIRONMENTAL DESIGN</td>
      <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
      <td>COMPLETED</td>
      <td>ENVIRONMENTAL HEALTH</td>
      <td>45202</td>
      <td>15483</td>
      <td>13761</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>4</td>
      <td>198</td>
      <td>173</td>
      <td>173</td>
      <td>210</td>
      <td>8</td>
      <td>23</td>
      <td>8</td>
      <td>156</td>
      <td>1</td>
      <td>174</td>
      <td>173</td>
      <td>406</td>
      <td>174</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
import sqlalchemy
import psycopg2
engine = sqlalchemy.create_engine('postgresql://dre:password@localhost:5432/cincinnati')
#chcc_activities.to_sql('chcc_activities', engine)
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
        <th>address</th>
        <th>category</th>
        <th>description</th>
        <th>implementation_date</th>
        <th>intervention</th>
        <th>latitude</th>
        <th>location</th>
        <th>longitude</th>
        <th>neighborhood</th>
        <th>notes</th>
        <th>outcome</th>
        <th>partners</th>
        <th>status</th>
        <th>topics</th>
        <th>zip_code</th>
        <th>zipcode_pop_2010</th>
        <th>zipcode_pop_2016</th>
    </tr>
    <tr>
        <td>3</td>
        <td>12191 MOSTELLER RD</td>
        <td>HEALTHY EATING</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>SYSTEMS</td>
        <td>39.2982939357563</td>
        <td>SHARONVILLE (UDF LOCATION)</td>
        <td>-84.4287401442128</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>None</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>COMPLETED</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>45241</td>
        <td>23044</td>
        <td>23707</td>
    </tr>
    <tr>
        <td>4</td>
        <td>1710 COLUMBUS AV</td>
        <td>HEALTHY EATING</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>SYSTEMS</td>
        <td>39.0769917000299</td>
        <td>WASHINGTON COURT HOUSE (UDF LOCATION)</td>
        <td>-84.3418176</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>None</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>COMPLETED</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>43160</td>
        <td>22305</td>
        <td>22171</td>
    </tr>
    <tr>
        <td>9</td>
        <td>735 E MAIN ST</td>
        <td>HEALTHY EATING</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>SYSTEMS</td>
        <td>39.9576447000294</td>
        <td>WEST UNION (UDF LOCATION)</td>
        <td>-82.9797113</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>None</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>COMPLETED</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>45693</td>
        <td>8981</td>
        <td>8305</td>
    </tr>
    <tr>
        <td>11</td>
        <td>509 LICKING PIKE</td>
        <td>HEALTHY EATING</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>SYSTEMS</td>
        <td>39.0553626000299</td>
        <td>WILDER (UDF LOCATION)</td>
        <td>-84.4866546</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>None</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>COMPLETED</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>41071</td>
        <td>20869</td>
        <td>20724</td>
    </tr>
    <tr>
        <td>12</td>
        <td>7224 TAYLORSVILLE RD</td>
        <td>HEALTHY EATING</td>
        <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE &quot;TO GO&quot; CUPS AND SALADS) AT UDF</td>
        <td>2016-04-01T00:00:00.000</td>
        <td>SYSTEMS</td>
        <td>39.8605103000295</td>
        <td>HUBER HEIGHTS (UDF LOCATION)</td>
        <td>-84.105625</td>
        <td>OUTSIDE CITY LIMITS</td>
        <td>None</td>
        <td>UDF HEALTHY FOOD RETAIL</td>
        <td>UNITED DAIRY FARMERS, OHIO DEPARTMENT OF HEALTH</td>
        <td>COMPLETED</td>
        <td>SMALL RETAIL/SUPERMARKETS</td>
        <td>45424</td>
        <td>49930</td>
        <td>50670</td>
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
    



## A Long Table


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
      <th>5</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>6</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>7</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>8</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>9</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>10</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>11</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>12</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>13</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>14</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>15</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>16</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>17</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>18</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>19</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>20</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>21</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>22</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>23</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>24</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>25</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>26</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>27</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>28</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>29</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>143</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>144</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>145</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>146</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>147</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>148</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>149</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>150</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>151</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>152</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>153</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>154</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>155</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>156</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>157</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>158</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>159</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>160</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>161</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>162</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>163</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>164</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>165</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>166</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
    </tr>
    <tr>
      <th>167</th>
      <td>HEALTHY FOOD OPTIONS (FRUIT AND VEGETABLE "TO ...</td>
      <td>SYSTEMS</td>
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





## A Bar Plot


```python
import matplotlib.pyplot as plt
plt.figure(figsize=(18,7))
chart = %sql SELECT category, count(category) FROM chcc_activities GROUP BY category
chart.bar()
```

     * postgresql://dre:***@localhost:5432/cincinnati
    3 rows affected.
    




    <BarContainer object of 3 artists>






```python
import matplotlib.pyplot as plt
plt.figure(figsize=(18,7))
chart = %sql SELECT topics, count(topics) FROM chcc_activities GROUP BY topics
chart.bar()
```

     * postgresql://dre:***@localhost:5432/cincinnati
    8 rows affected.
    




    <BarContainer object of 8 artists>




![png](./index_21_2.png)


jupyter nbconvert index.ipynb --to markdown --NbConvertApp.output_files_dir=.

# References

1. [Creating Healthy Communities Coalition (CHCC) Activities](https://dev.socrata.com/foundry/data.cincinnati-oh.gov/skqm-k58y)
1. [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/index.html)
1. [SQLAlchemy — Python Tutorial](https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91)
1. [How to write DataFrame to postgres table?](https://stackoverflow.com/questions/23103962/how-to-write-dataframe-to-postgres-table)
1. [Python: Get the Python version](https://www.w3resource.com/python-exercises/python-basic-exercise-2.php)
1. [Python Select from PostgreSQL Table](https://pynative.com/python-postgresql-select-data-from-table/)
1. [Here’s How to Run SQL in Jupyter Notebooks](https://towardsdatascience.com/heres-how-to-run-sql-in-jupyter-notebooks-f26eb90f3259)
