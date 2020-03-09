---
title: City of Cincinnati Expenses 
subtitle: An exploration of the City of Cincinnati's expenses.  
summary: The source of this data set is the City of Cincinnati Financial System (CFS) 
authors:
- andre
tags: [Jupyter, Fiscal Sustainability]
categories: [Python, Finance, SQL]
date: "2020-03-04T00:00:00Z"
lastMod: "2020-03-04T00:00:00Z"
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
{{% toc %}}


```python
# Getting the Data

[City of Cincinnati Expenses by Object Code](https://dev.socrata.com/foundry/data.cincinnati-oh.gov/55e3-bbia)
```


```python
#!/usr/bin/env python

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
results = client.get("55e3-bbia", limit=1813010)

# Convert to pandas DataFrame
cinci_exp = pd.DataFrame.from_records(results)
```

    WARNING:root:Requests made without an app_token will be subject to strict throttling limits.



```python
print("Summary of the basic information about this DataFrame and its data:")
print(cinci_exp.info())
```

    Summary of the basic information about this DataFrame and its data:
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 813010 entries, 0 to 813009
    Data columns (total 13 columns):
     #   Column               Non-Null Count   Dtype 
    ---  ------               --------------   ----- 
     0   fiscal_year          813010 non-null  object
     1   bfy                  813010 non-null  object
     2   fund_code            813010 non-null  object
     3   fund_name            813010 non-null  object
     4   department           813010 non-null  object
     5   department_name      813010 non-null  object
     6   object_code          813010 non-null  object
     7   object_name          813010 non-null  object
     8   expenses             813010 non-null  object
     9   prior_yr_expenses    813010 non-null  object
     10  doc_rec_dt           813010 non-null  object
     11  commodity_code       298560 non-null  object
     12  commodity_code_desc  298560 non-null  object
    dtypes: object(13)
    memory usage: 80.6+ MB
    None



```python
cinci_exp.describe(include='all')
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
      <th>fiscal_year</th>
      <th>bfy</th>
      <th>fund_code</th>
      <th>fund_name</th>
      <th>department</th>
      <th>department_name</th>
      <th>object_code</th>
      <th>object_name</th>
      <th>expenses</th>
      <th>prior_yr_expenses</th>
      <th>doc_rec_dt</th>
      <th>commodity_code</th>
      <th>commodity_code_desc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>813010</td>
      <td>298560</td>
      <td>298560</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>6</td>
      <td>9</td>
      <td>179</td>
      <td>179</td>
      <td>150</td>
      <td>147</td>
      <td>306</td>
      <td>303</td>
      <td>365125</td>
      <td>52763</td>
      <td>1782</td>
      <td>2322</td>
      <td>2286</td>
    </tr>
    <tr>
      <th>top</th>
      <td>2015</td>
      <td>2015</td>
      <td>050</td>
      <td>GENERAL</td>
      <td>252</td>
      <td>TRAFFIC AND ROAD OPERATIONS</td>
      <td>7289</td>
      <td>EXPERT SERVICES-NOC</td>
      <td>0</td>
      <td>0</td>
      <td>2018-11-27T00:00:00.000</td>
      <td>98526</td>
      <td>COPY MACHINE (INCL. COST-PER-COPY TYPE LEASES)...</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>145690</td>
      <td>144753</td>
      <td>229711</td>
      <td>229711</td>
      <td>31365</td>
      <td>31365</td>
      <td>58718</td>
      <td>58718</td>
      <td>48666</td>
      <td>733588</td>
      <td>3029</td>
      <td>10700</td>
      <td>10700</td>
    </tr>
  </tbody>
</table>
</div>




```python
import sqlalchemy
import psycopg2
engine = sqlalchemy.create_engine('postgresql://dre:password@localhost:5432/cincinnati')
#cinci_exp.to_sql('cinci_exp', engine)
```


```python
connection = psycopg2.connect(user="dre",
                                  password="password",
                                  host="localhost",
                                  port="5432",
                                  database="cincinnati")
cursor = connection.cursor()
postgreSQL_select_Query = "SELECT * FROM cinci_exp"

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
SELECT column_name, data_type 
FROM information_schema.columns 
WHERE table_name = 'cinci_exp';
```

     * postgresql://dre:***@localhost:5432/cincinnati
    14 rows affected.





<table>
    <tr>
        <th>column_name</th>
        <th>data_type</th>
    </tr>
    <tr>
        <td>index</td>
        <td>bigint</td>
    </tr>
    <tr>
        <td>fiscal_year</td>
        <td>text</td>
    </tr>
    <tr>
        <td>bfy</td>
        <td>text</td>
    </tr>
    <tr>
        <td>fund_code</td>
        <td>text</td>
    </tr>
    <tr>
        <td>fund_name</td>
        <td>text</td>
    </tr>
    <tr>
        <td>department</td>
        <td>text</td>
    </tr>
    <tr>
        <td>department_name</td>
        <td>text</td>
    </tr>
    <tr>
        <td>object_code</td>
        <td>text</td>
    </tr>
    <tr>
        <td>object_name</td>
        <td>text</td>
    </tr>
    <tr>
        <td>expenses</td>
        <td>text</td>
    </tr>
    <tr>
        <td>prior_yr_expenses</td>
        <td>text</td>
    </tr>
    <tr>
        <td>doc_rec_dt</td>
        <td>text</td>
    </tr>
    <tr>
        <td>commodity_code</td>
        <td>text</td>
    </tr>
    <tr>
        <td>commodity_code_desc</td>
        <td>text</td>
    </tr>
</table>




```python
%%sql 
SELECT 
  COUNT(*) 
    FROM (
      SELECT DISTINCT *
        FROM cinci_exp) 
        AS unique_foodsafety;
```

     * postgresql://dre:***@localhost:5432/cincinnati
    1 rows affected.





<table>
    <tr>
        <th>count</th>
    </tr>
    <tr>
        <td>813010</td>
    </tr>
</table>




```python
%%sql 
SELECT *
FROM cinci_exp
WHERE fiscal_year =  '2020'
ORDER BY expenses
LIMIT 5
```

     * postgresql://dre:***@localhost:5432/cincinnati
    5 rows affected.





<table>
    <tr>
        <th>index</th>
        <th>fiscal_year</th>
        <th>bfy</th>
        <th>fund_code</th>
        <th>fund_name</th>
        <th>department</th>
        <th>department_name</th>
        <th>object_code</th>
        <th>object_name</th>
        <th>expenses</th>
        <th>prior_yr_expenses</th>
        <th>doc_rec_dt</th>
        <th>commodity_code</th>
        <th>commodity_code_desc</th>
    </tr>
    <tr>
        <td>5809</td>
        <td>2020</td>
        <td>2020</td>
        <td>302</td>
        <td>INCOME TAX INFRASTRUCTURE</td>
        <td>233</td>
        <td>DIVISION OF ENGINEERING</td>
        <td>7131</td>
        <td>SHIFT DIFFERENTIAL</td>
        <td>0</td>
        <td>0</td>
        <td>2019-07-09T00:00:00.000</td>
        <td>None</td>
        <td>None</td>
    </tr>
    <tr>
        <td>3600</td>
        <td>2020</td>
        <td>2019</td>
        <td>701</td>
        <td>METROPOLITAN SEWER DISTRICT</td>
        <td>450</td>
        <td>MSD DIV OF WASTEWATER COLLECTION</td>
        <td>7289</td>
        <td>EXPERT SERVICES-NOC</td>
        <td>0</td>
        <td>0</td>
        <td>2019-12-31T00:00:00.000</td>
        <td>None</td>
        <td>None</td>
    </tr>
    <tr>
        <td>5546</td>
        <td>2020</td>
        <td>2020</td>
        <td>350</td>
        <td>PUBLIC HEALTH RESEARCH</td>
        <td>261</td>
        <td>HEALTH, OFFICE OF THE COMMISSIONER</td>
        <td>7289</td>
        <td>EXPERT SERVICES-NOC</td>
        <td>0</td>
        <td>0</td>
        <td>2019-07-09T00:00:00.000</td>
        <td>None</td>
        <td>None</td>
    </tr>
    <tr>
        <td>5655</td>
        <td>2020</td>
        <td>2020</td>
        <td>104</td>
        <td>GENERAL AVIATION</td>
        <td>234</td>
        <td>DIV OF AVIATION</td>
        <td>7214</td>
        <td>TRAVEL EXPENSE NON-LOCAL</td>
        <td>0</td>
        <td>0</td>
        <td>2019-07-09T00:00:00.000</td>
        <td>None</td>
        <td>None</td>
    </tr>
    <tr>
        <td>7924</td>
        <td>2020</td>
        <td>2020</td>
        <td>302</td>
        <td>INCOME TAX INFRASTRUCTURE</td>
        <td>233</td>
        <td>DIVISION OF ENGINEERING</td>
        <td>7114</td>
        <td>COMPENSATORY TIME USED</td>
        <td>0</td>
        <td>0</td>
        <td>2019-07-09T00:00:00.000</td>
        <td>None</td>
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




![png](./index_22_2.png)


jupyter nbconvert index.ipynb --to markdown --NbConvertApp.output_files_dir=.

# References

1. [Creating Healthy Communities Coalition (CHCC) Activities](https://dev.socrata.com/foundry/data.cincinnati-oh.gov/skqm-k58y)
1. [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/index.html)
1. [SQLAlchemy — Python Tutorial](https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91)
1. [How to write DataFrame to postgres table?](https://stackoverflow.com/questions/23103962/how-to-write-dataframe-to-postgres-table)
1. [Python: Get the Python version](https://www.w3resource.com/python-exercises/python-basic-exercise-2.php)
1. [Python Select from PostgreSQL Table](https://pynative.com/python-postgresql-select-data-from-table/)
1. [Here’s How to Run SQL in Jupyter Notebooks](https://towardsdatascience.com/heres-how-to-run-sql-in-jupyter-notebooks-f26eb90f3259)
