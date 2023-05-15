# Sexual Offense in US

### Inequality is always a hot topic in anywhere of the world, espeically in the U.S.. However, every time when people talk about inequality of the minority, they talk about race, mostly black, while ignoring a large portion of population who has been mistreated for decades----females. Inequality or stereotype of females exist since young. Sexual directive prints on baby onesies, stereotypical career advice for females, unequal opportunities  in academics, unreasonable limitations for females in workplaces, etc.. They exist everywhere; I can say no girl could say that she has not been mistreated in her entire life. 
### Not everyone can name a person around them who are victim or offender of sexual offense, but it's easy to find a real-life example, or to say, criminal case. Notorious rapist Jeffery Eiberstain and Warren S. Jeffs of Keep Sweet: Pray and Obey; I can already name two just in US. Sexual offense is the worst form of malice agianst women. This project is tend to see analyze the victim + offender proflie of sexual offenses and dig deeper into the demographical factors that might correlate with seuxal offense, specifically in 2021.


```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import re
import seaborn as sns
import geopandas as gpd
import plotly.express as px

# add leading description
```


```python
raw = pd.read_excel("./data/trend.xls", skiprows = 3, skipfooter = 7)
```


```python
raw.columns
```




    Index(['Year', 'Population1', 'Violent\ncrime2', 'Violent \ncrime \nrate ',
           'Murder and\nnonnegligent \nmanslaughter',
           'Murder and \nnonnegligent \nmanslaughter \nrate ',
           'Rape\n(revised \ndefinition)3', 'Rape\n(revised \ndefinition) \nrate3',
           'Rape\n(legacy \ndefinition)4', 'Rape\n(legacy \ndefinition) \nrate4',
           'Robbery', 'Robbery \nrate ', 'Aggravated \nassault',
           'Aggravated \nassault rate ', 'Property \ncrime',
           'Property \ncrime \nrate ', 'Burglary', 'Burglary \nrate ',
           'Larceny-\ntheft', 'Larceny-\ntheft rate ', 'Motor \nvehicle \ntheft',
           'Motor \nvehicle \ntheft \nrate ', 'Unnamed: 22', 'Unnamed: 23'],
          dtype='object')




```python
raw_13=raw.loc[:, ["Year", "Rape\n(revised \ndefinition)3", "Rape\n(revised \ndefinition) \nrate3", "Rape\n(legacy \ndefinition)4", "Rape\n(legacy \ndefinition) \nrate4"]]
raw_13.columns = ["year", "revised rape","revised rape rate", "legacy rape", "legacy rape rate"]
```


```python
rape = raw_13[13:]
rape["year"].iloc[5] = 2018
rape["revised rape"] = pd.to_numeric(rape["revised rape"])
rape
```

    /Users/yahanchen/opt/anaconda3/lib/python3.8/site-packages/pandas/core/indexing.py:1637: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self._setitem_single_block(indexer, value, name)
    /Users/yahanchen/opt/anaconda3/lib/python3.8/site-packages/pandas/core/indexing.py:692: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      iloc._setitem_with_indexer(indexer, value, self.name)
    <ipython-input-5-41cc407a6476>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      rape["revised rape"] = pd.to_numeric(rape["revised rape"])





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
      <th>year</th>
      <th>revised rape</th>
      <th>revised rape rate</th>
      <th>legacy rape</th>
      <th>legacy rape rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>2013</td>
      <td>113695</td>
      <td>35.9</td>
      <td>82109</td>
      <td>25.9</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2014</td>
      <td>118027</td>
      <td>37.0</td>
      <td>84864</td>
      <td>26.6</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2015</td>
      <td>126134</td>
      <td>39.3</td>
      <td>91261</td>
      <td>28.4</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2016</td>
      <td>132414</td>
      <td>40.9</td>
      <td>96970</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2017</td>
      <td>135666</td>
      <td>41.7</td>
      <td>99708</td>
      <td>30.7</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2018</td>
      <td>143765</td>
      <td>44.0</td>
      <td>101363</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2019</td>
      <td>139815</td>
      <td>42.6</td>
      <td>98213</td>
      <td>29.9</td>
    </tr>
  </tbody>
</table>
</div>



## As the very beginning, it's important to see the trend of sexual offense cases in US in previous years, from 2013-2019.
-note: After 2019, rape is categorized as sexual offense under violent crime. They share the same definition.

## Observe a General Trend: Number of Rape Cases from 2013-2019 in US


```python
plt.figure(figsize=(10,7))
plt.plot(rape["year"], rape["revised rape"], label = "revised definition")
plt.plot(rape["year"], rape["legacy rape"], label = "legacy definition")
plt.legend(loc='best')

plt.xlabel("year", fontsize = 12)
plt.ylabel("number of cases", fontsize = 12)
plt.title("Number of Rape Cases in US from 2013-2019")
# sns.lineplot(rape["year"], rape["legacy rape"])
# sns.lineplot(rape["year"], rape["revised rape"])
```




    Text(0.5, 1.0, 'Number of Rape Cases in US from 2013-2019')




    
![png](output_7_1.png)
    



```python
# rape["revised rape"] = pd.to_numeric(rape["revised rape"])
# sns.lineplot(data = rape, x = "year", y = "legacy rape", label = "legacy rape")
# sns.lineplot(data = rape, x = "year", y = "revised rape", label = "revised rape")

# sns.lineplot(x = "year", data=rape[["year", "legacy rape", "revised rape"]])
```

### The revised definition of rape includes both female and male victims (rape), sodomy, and sextual assult with an object, where are legacy definition only includes female victims (rape). The legacy rape rate increased 15.4% from 2013 to 2019.


## Data Preparation


```python
# sex_rate = the number of reported sexual assults per 100,000 people
# data scource: FBI Crime Data Explorer
so = pd.read_csv('./data/2021_states.csv', header=0, index_col=0)
so_2021 = pd.read_excel("./data/state_sex.xlsx", skiprows = 5, skipfooter = 1)
```


```python
#state abbr id
us_state_to_abbrev = {
    "Alabama": "AL",
    "Alaska": "AK",
    "Arizona": "AZ",
    "Arkansas": "AR",
    "California": "CA",
    "Colorado": "CO",
    "Connecticut": "CT",
    "Delaware": "DE",
    "Florida": "FL",
    "Georgia": "GA",
    "Hawaii": "HI",
    "Idaho": "ID",
    "Illinois": "IL",
    "Indiana": "IN",
    "Iowa": "IA",
    "Kansas": "KS",
    "Kentucky": "KY",
    "Louisiana": "LA",
    "Maine": "ME",
    "Maryland": "MD",
    "Massachusetts": "MA",
    "Michigan": "MI",
    "Minnesota": "MN",
    "Mississippi": "MS",
    "Missouri": "MO",
    "Montana": "MT",
    "Nebraska": "NE",
    "Nevada": "NV",
    "New Hampshire": "NH",
    "New Jersey": "NJ",
    "New Mexico": "NM",
    "New York": "NY",
    "North Carolina": "NC",
    "North Dakota": "ND",
    "Ohio": "OH",
    "Oklahoma": "OK",
    "Oregon": "OR",
    "Pennsylvania": "PA",
    "Rhode Island": "RI",
    "South Carolina": "SC",
    "South Dakota": "SD",
    "Tennessee": "TN",
    "Texas": "TX",
    "Utah": "UT",
    "Vermont": "VT",
    "Virginia": "VA",
    "Washington": "WA",
    "West Virginia": "WV",
    "Wisconsin": "WI",
    "Wyoming": "WY",
    "District of Columbia": "DC",
    "American Samoa": "AS",
    "Guam": "GU",
    "Northern Mariana Islands": "MP",
    "Puerto Rico": "PR",
    "United States Minor Outlying Islands": "UM",
    "U.S. Virgin Islands": "VI",
}

# apply dict + add sexual offense rate data
so['state_codes'] = so['state'].replace(us_state_to_abbrev)
so['reported_rape_cases'] = so_2021["Sex\nOffenses"]

rate_state = pd.read_excel("./data/sexrate21.xlsx").dropna(axis = "columns")
rate_state["sex_rate"] = (rate_state["sex offenses"]/rate_state["population"]*100000).round(1)
rate_state.drop(columns = ["sex offenses", "population"])
sexual_factors = so.merge(rate_state, how = "left", on = "state").rename(columns = {"avg_salary": "salary"})
sexual_factors["year"] = 2021
sexual_factors.sort_values("sex_rate", ascending = False).head()
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
      <th>state</th>
      <th>reported_rape_cases</th>
      <th>rape_arrests</th>
      <th>sex_offense_arrests</th>
      <th>salary</th>
      <th>score.x</th>
      <th>score.y</th>
      <th>policing_correction_spend_per_capita</th>
      <th>policing_correction_expenditure</th>
      <th>correction_spending_per</th>
      <th>state_codes</th>
      <th>sex offenses</th>
      <th>population</th>
      <th>sex_rate</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>44</th>
      <td>Utah</td>
      <td>5134</td>
      <td>291</td>
      <td>498</td>
      <td>54678</td>
      <td>55.53</td>
      <td>20.3</td>
      <td>484</td>
      <td>1553</td>
      <td>68%</td>
      <td>UT</td>
      <td>5134</td>
      <td>3337975</td>
      <td>153.8</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Montana</td>
      <td>1680</td>
      <td>62</td>
      <td>118</td>
      <td>52135</td>
      <td>51.55</td>
      <td>55.1</td>
      <td>608</td>
      <td>649</td>
      <td>74%</td>
      <td>MT</td>
      <td>1680</td>
      <td>1104271</td>
      <td>152.1</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>34</th>
      <td>North Dakota</td>
      <td>1121</td>
      <td>51</td>
      <td>81</td>
      <td>53525</td>
      <td>58.42</td>
      <td>25.5</td>
      <td>574</td>
      <td>437</td>
      <td>70%</td>
      <td>ND</td>
      <td>1121</td>
      <td>774948</td>
      <td>144.7</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>41</th>
      <td>South Dakota</td>
      <td>1155</td>
      <td>36</td>
      <td>51</td>
      <td>48984</td>
      <td>51.19</td>
      <td>34.2</td>
      <td>478</td>
      <td>423</td>
      <td>85%</td>
      <td>SD</td>
      <td>1155</td>
      <td>895376</td>
      <td>129.0</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>29</th>
      <td>New Hampshire</td>
      <td>1790</td>
      <td>71</td>
      <td>100</td>
      <td>59622</td>
      <td>59.19</td>
      <td>42.0</td>
      <td>525</td>
      <td>714</td>
      <td>45%</td>
      <td>NH</td>
      <td>1790</td>
      <td>1388992</td>
      <td>128.9</td>
      <td>2021</td>
    </tr>
  </tbody>
</table>
</div>



## Sexual Offense Rate Data: 2015-2020

#### Data from: UCR Crime database


```python
states = ["Alabama", "Alaska", "Arizona", "Arkansas", "California", "Colorado", 
          "Connecticut", "Delaware", "District of Columbia", "Florida", "Georgia", 
          "Hawaii", "Idaho", "Illinois", "Indiana", "Iowa", "Kansas", "Kentucky", 
          "Louisiana", "Maine", "Maryland", "Massachusetts", "Michigan", "Minnesota", 
          "Mississippi", "Missouri", "Montana", "Nebraska", "Nevada", "New Hampshire", 
          "New Jersey", "New Mexico", "New York", "North Carolina", "North Dakota", 
          "Ohio", "Oklahoma", "Oregon", "Pennsylvania", "Rhode Island", "South Carolina", 
          "South Dakota", "Tennessee", "Texas", "Utah", "Vermont", "Virginia", "Washington", 
          "West Virginia", "Wisconsin", "Wyoming"]
```


```python
def sex_data_clean(path, year):
    df = pd.read_excel(path, skiprows=3, skipfooter=8)
    df = df[df['Unnamed: 2'] == "Rate per 100,000 inhabitants"]
    df = df[[df.columns[6]]].rename(columns = {df.columns[6]: "sex_rate"})
    df = df[df["sex_rate"] != df["sex_rate"].min()]
    df["state"] = states
    df["year"] = year
    
    return df

sex_rate15 = sex_data_clean("./data/rape15.xls", 2015) 
sex_rate16 = sex_data_clean("./data/rape16.xls", 2016)
```


```python
def sex_data_later(path, year):
    df1 = pd.read_excel(path, skiprows=3, skipfooter=5)
    df1 = df1[df1['Unnamed: 2'] == "Rate per 100,000 inhabitants"]
    df1 = df1[[df1.columns[6]]].rename(columns = {df1.columns[6]: "sex_rate"})
    df1 = df1[df1["sex_rate"] != df1["sex_rate"].min()]
    df1["state"] = states
    df1["year"] = year
    
    return df1

sex_rate17 = sex_data_later("./data/rape17.xls", 2017) 
sex_rate18 = sex_data_later("./data/rape18.xls", 2018) 
sex_rate19 = sex_data_later("./data/rape19.xls", 2019)
sex_rate20 = pd.read_excel("./data/rate_sex20.xlsx")
sex_rate20["year"] = 2020
```


```python
sex_list = [sex_rate15, sex_rate16, sex_rate17, sex_rate18, sex_rate19, sex_rate20]
offense_rate = pd.concat(sex_list)
offense_rate = offense_rate[offense_rate["state"] != "United States"]
offense_rate.head()
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
      <th>sex_rate</th>
      <th>state</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>42.0</td>
      <td>Alabama</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>19</th>
      <td>122.0</td>
      <td>Alaska</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>29</th>
      <td>45.5</td>
      <td>Arizona</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>40</th>
      <td>64.8</td>
      <td>Arkansas</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>49</th>
      <td>32.7</td>
      <td>California</td>
      <td>2015</td>
    </tr>
  </tbody>
</table>
</div>



## Police Correction Spending Data 2015-2020

##### data from: Bureau of Justice Statistics and US Census Bureau


```python
def clean_later(path): 
    data = pd.read_csv(path)
    data = data[["state", "police_correction_per_capita", "year"]]
    data = data.rename(columns = {"police_correction_per_capita": "policing_correction_spend_per_capita"})
    data["policing_correction_spend_per_capita"] = data["policing_correction_spend_per_capita"].round(2)
    return data
    
police18 = clean_later("./data/police_correction_18.csv")
police19 = clean_later("./data/police_correction_19.csv")
police20 = clean_later("./data/police_correction_20.csv")
```


```python
police17 = pd.read_csv("./data/jeeus17/capita17.csv", skiprows = 10, skipfooter = 3, engine = "python").drop(columns=["Unnamed: 1",
                                                                                                                      "2017 population",
                                                                                                                      "All government functions",
                                                                                                                      "Total justice system",
                                                                                                                      "Judicial and legal functions"]).dropna()
police17 = police17.rename(columns = {"Jurisdiction": "state"})
police17["Police protection"] = police17["Police protection"].str.replace('$', '', regex = True).astype(int)
police17["Corrections"] = police17["Corrections"].str.replace('$', '', regex = True).astype(int)

police17["policing_correction_spend_per_capita"] = police17["Police protection"] + police17["Corrections"]
police17["year"] = 2017
police17 = police17.drop(columns = ["Police protection", "Corrections"])

```


```python
def clean_police_data(path, year):
    df = pd.read_csv(path, skiprows = 11, skipfooter = 1, engine = "python")
    df = df[["State", "Police protection per capita expenditure", "Corrections per capita expenditure"]].drop([0])
    df = df.rename(columns = {"State": "state"})
    df["policing_correction_spend_per_capita"] = df["Police protection per capita expenditure"] + df["Corrections per capita expenditure"]
    df = df.drop(columns = ["Police protection per capita expenditure", "Corrections per capita expenditure"])
    df["year"] = year
    
    return df
    
police16 = clean_police_data("./data/jeee16f/capita16.csv", 2016)

```


```python
police15 = clean_police_data("./data/jeee15fu/capita15.csv", 2015)
police14 = clean_police_data("./data/jeee14fu/capita14.csv", 2014)
```


```python
police13 = pd.read_csv("./data/jeee13f/capita13.csv", skipfooter = 1, engine = "python")
police13 = police13.rename(columns = {"State": "state"})
police13 = police13[["state", "Police Protection", "Corrections"]]
police13["policing_correction_spend_per_capita"] = police13["Police Protection"] + police13["Corrections"]
police13["year"] = 2013
police13 = police13.drop(columns = ["Police Protection", "Corrections"])

```


```python
police_list = [police15, police16, police17, police18, police19, police20]
police_correct = pd.concat(police_list)
police_correct
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
      <th>state</th>
      <th>policing_correction_spend_per_capita</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>408.21</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alaska</td>
      <td>1008.08</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arizona</td>
      <td>556.23</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Arkansas</td>
      <td>404.77</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>5</th>
      <td>California</td>
      <td>817.37</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Virginia</td>
      <td>648.70</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Washington</td>
      <td>600.46</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>48</th>
      <td>West Virginia</td>
      <td>491.23</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Wisconsin</td>
      <td>596.28</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Wyoming</td>
      <td>784.72</td>
      <td>2020</td>
    </tr>
  </tbody>
</table>
<p>306 rows × 3 columns</p>
</div>



## Teacher Salary Data 2015-2020

#### Data from: National Center for Education Statistics


```python
salary_1516 = pd.read_excel("./data/salary2016.xls", skiprows = 2, skipfooter = 4).drop([0, 1]).dropna()
salary_1516 = salary_1516.rename(columns = {"Unnamed: 0": "state", "2014-15.1": "salary-2015", "2015-16.1": "salary-2016"})
salary_1516 = salary_1516[["state", "salary-2015", "salary-2016"]]
salary_1516["state"] = salary_1516["state"].str.replace(r".", r"", regex=False) #remove dots
salary_1516["id"] = range(0, len(salary_1516["state"]))
salary_1516 = pd.wide_to_long(salary_1516, stubnames='salary', i="id", j='year',
                    sep='-', suffix=r'\w+').reset_index().drop(columns = ["id"])
salary_1516["state"] = salary_1516["state"].str.rstrip()
salary_1516.loc[8, "state"] = "District of Columbia"
salary_1516.loc[59, "state"] = "District of Columbia"
salary_1516.head(10)

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
      <th>year</th>
      <th>state</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015</td>
      <td>Alabama</td>
      <td>48939.435703</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015</td>
      <td>Alaska</td>
      <td>67206.023952</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015</td>
      <td>Arizona</td>
      <td>45712.781418</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015</td>
      <td>Arkansas</td>
      <td>48146.111654</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015</td>
      <td>California</td>
      <td>73025.075984</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2015</td>
      <td>Colorado</td>
      <td>50164.658250</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2015</td>
      <td>Connecticut</td>
      <td>72193.495192</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015</td>
      <td>Delaware</td>
      <td>59594.945515</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2015</td>
      <td>District of Columbia</td>
      <td>76000.041167</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2015</td>
      <td>Florida</td>
      <td>49323.009894</td>
    </tr>
  </tbody>
</table>
</div>




```python
salary_1718 = pd.read_excel("./data/salary2018.xls", skiprows = 2, skipfooter = 3).drop([0, 1]).dropna()
salary_1718 = salary_1718.rename(columns = {"Unnamed: 0": "state", "2016-17.1": "salary-2017", "2017-18.1": "salary-2018"})
salary_1718 = salary_1718[["state", "salary-2017", "salary-2018"]]
salary_1718["state"] = salary_1718["state"].str.replace(r".", r"", regex=False)
salary_1718["id"] = range(0, len(salary_1718["state"]))
salary_1718 = pd.wide_to_long(salary_1718, stubnames='salary', i="id", j='year',
                    sep='-', suffix=r'\w+').reset_index().drop(columns = ["id"])
salary_1718["state"] = salary_1718["state"].str.rstrip()
salary_1718.loc[8, "state"] = "District of Columbia"
salary_1718.loc[59, "state"] = "District of Columbia"
#salary_1718

```


```python
salary_1920 = pd.read_excel("./data/salary19-20.xls", skiprows = 2, skipfooter = 3).drop([0, 1]).dropna()
salary_1920 = salary_1920.rename(columns = {"Unnamed: 0": "state", "2018-19.1": "salary-2019", "2019-20.1": "salary-2020"})
salary_1920 = salary_1920[["state", "salary-2019", "salary-2020"]]
salary_1920["state"] = salary_1920["state"].str.replace(r".", r"", regex=False)
salary_1920["id"] = range(0, len(salary_1920["state"]))
salary_1920 = pd.wide_to_long(salary_1920, stubnames='salary', i="id", j='year',
                    sep='-', suffix=r'\w+').reset_index().drop(columns = ["id"])
salary_1920["state"] = salary_1920["state"].str.rstrip()
salary_1920.loc[8, "state"] = "District of Columbia"
salary_1920.loc[59, "state"] = "District of Columbia"
salary_1920.head(10)

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
      <th>year</th>
      <th>state</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019</td>
      <td>Alabama</td>
      <td>52822.603211</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019</td>
      <td>Alaska</td>
      <td>71376.378816</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019</td>
      <td>Arizona</td>
      <td>51140.697561</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019</td>
      <td>Arkansas</td>
      <td>50211.383752</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019</td>
      <td>California</td>
      <td>84358.334136</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019</td>
      <td>Colorado</td>
      <td>55794.376115</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019</td>
      <td>Connecticut</td>
      <td>77661.180844</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019</td>
      <td>Delaware</td>
      <td>64657.897010</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2019</td>
      <td>District of Columbia</td>
      <td>79704.655582</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2019</td>
      <td>Florida</td>
      <td>49069.800449</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_list = [salary_1516, salary_1718, salary_1920]
teacher_salary = pd.concat(df_list)
teacher_salary["salary"] = teacher_salary["salary"].round(2)
teacher_salary.head()
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
      <th>year</th>
      <th>state</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015</td>
      <td>Alabama</td>
      <td>48939.44</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015</td>
      <td>Alaska</td>
      <td>67206.02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015</td>
      <td>Arizona</td>
      <td>45712.78</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015</td>
      <td>Arkansas</td>
      <td>48146.11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015</td>
      <td>California</td>
      <td>73025.08</td>
    </tr>
  </tbody>
</table>
</div>



## State GDP Data: 2015-2020


```python
gdp1517=pd.read_csv("./data/gdp13-17.csv").drop(columns = ["Fips"]).rename(columns = {"Area": "state",
                                                                                     "2015": "gdp-2015", 
                                                                                     "2016": "gdp-2016",
                                                                                     "2017": "gdp-2017"})
gdp1517 = gdp1517[["state", "gdp-2015", "gdp-2016", "gdp-2017"]]
gdp1517 = gdp1517[gdp1517["state"] != "United States"]
gdp1517.drop(gdp1517.tail(8).index, inplace = True)
gdp1517["id"] = range(0, len(gdp1517["state"]))
gdp1517 = pd.wide_to_long(gdp1517, stubnames='gdp', i="id", j='year',
                    sep='-', suffix=r'\w+').reset_index().drop(columns = ["id"])
gdp1517 = gdp1517.rename(columns = {"gdp": "gdp_capita"})
gdp1517.head()
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
      <th>year</th>
      <th>state</th>
      <th>gdp_capita</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015</td>
      <td>Alabama</td>
      <td>36818</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015</td>
      <td>Alaska</td>
      <td>65971</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015</td>
      <td>Arizona</td>
      <td>38787</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015</td>
      <td>Arkansas</td>
      <td>36295</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015</td>
      <td>California</td>
      <td>57637</td>
    </tr>
  </tbody>
</table>
</div>




```python
def gdp_data(path, year): 
    df = pd.read_excel(path, skiprows = 5, skipfooter = 3).drop(columns = ["GeoFips"]).rename(columns = 
                                                                                                       {"GeoName": "state"})
    df["gdp_capita"] = (df["gdp"]*1000000/df["pop"]).round(2)
    df = df[["state", "gdp_capita"]]
    df["year"] = year
    
    return df
gdp18 = gdp_data("./data/gdp18.xlsx", 2018)
gdp19 = gdp_data("./data/gdp19.xlsx", 2019)
gdp20 = gdp_data("./data/gdp20.xlsx", 2020)

gdp21 = pd.read_excel("./data/gdp21.xlsx", skiprows = 4).sort_values(by = "state")
gdp21["year"] = 2021

```


```python
gdp_list = [gdp18, gdp19, gdp20, gdp1517]
gdp = pd.concat(gdp_list)
gdp.head()
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
      <th>state</th>
      <th>gdp_capita</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>40962.35</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>72393.79</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>43944.37</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>38472.45</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>67044.40</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
</div>



## Poverty Rate: 2015-2020

#### Data from: kaggle https://www.kaggle.com/code/marshuu/poverty-rate-in-the-us-animation/input


```python
poverty = pd.read_csv("./data/poverty15-20.csv").drop(columns = ["Unnamed: 0"])
poverty1520 = poverty[poverty["state"] != "United States"]
poverty1520
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
      <th>state</th>
      <th>poverty_rate</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>18.5</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alaska</td>
      <td>10.4</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arizona</td>
      <td>17.4</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Arkansas</td>
      <td>18.7</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>5</th>
      <td>California</td>
      <td>15.4</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>307</th>
      <td>Virginia</td>
      <td>9.2</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>308</th>
      <td>Washington</td>
      <td>9.5</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>309</th>
      <td>West Virginia</td>
      <td>15.8</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>310</th>
      <td>Wisconsin</td>
      <td>10.0</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>311</th>
      <td>Wyoming</td>
      <td>9.2</td>
      <td>2020</td>
    </tr>
  </tbody>
</table>
<p>306 rows × 3 columns</p>
</div>



## Education Spending Purpil: 2016-2021

#### Data from: US Census Bureau--Public Elementary-Secondary Education Finance Data


```python
def clean_eduspend(path, year): 
    raw = pd.read_excel(path, sheet_name = "8", skiprows = 8, skipfooter = 6)
    raw = raw[["Unnamed: 0", "Total 1  "]].dropna(axis = 0).rename(columns = {"Unnamed: 0": "state",
                                                                             "Total 1  ": "edu_spending_per_pupil"})
    raw["state"] = raw["state"].str.replace(r".", r"", regex=False)
    raw = raw.drop(1)
    raw["year"] = year
    
    return raw

edu_spend16 = clean_eduspend("./data/edu spend 16.xls", 2016)
edu_spend17 = clean_eduspend("./data/edu spend 17.xls", 2017)
edu_spend18 = clean_eduspend("./data/edu spend 18.xls", 2018)
edu_spend19 = clean_eduspend("./data/edu spend 19.xls", 2019)
edu_spend20 = clean_eduspend("./data/edu spend 20.xls", 2020)
edu_spend16.head()
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
      <th>state</th>
      <th>edu_spending_per_pupil</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Alabama</td>
      <td>9242.677695</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alaska</td>
      <td>17509.975316</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Arizona</td>
      <td>7613.006435</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Arkansas</td>
      <td>9845.568548</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>7</th>
      <td>California</td>
      <td>11495.363449</td>
      <td>2016</td>
    </tr>
  </tbody>
</table>
</div>



### Due to reporting timing, only 41 states are included in the report. I decide not to include the data in 2021


```python
# edu21 = pd.read_excel("./data/edu spend 21.xls", sheet_name = "Table 1", skiprows = 5, skipfooter = 9)
# edu21 = edu21[["Unnamed: 0", "Per pupil"]].dropna(axis = 0).rename(columns = {"Unnamed: 0": "state",
#                                                                              "Per pupil": "edu_spending_per_pupil"})
# edu21["state"] = edu21["state"].str.replace(r".", r"", regex=False)
# edu_spend21 = edu21.drop(3)
# edu_spend21["year"] = 2021
# edu21

edu_list = [edu_spend16, edu_spend17, edu_spend18, edu_spend19, edu_spend20]
edu_spending = pd.concat(edu_list)

```

## Drug Overdoes of Each State 2016-2021
#### Data from: CDC--Drug Overdose Mortality by State


```python
abbr_states = {
        'AK': 'Alaska',
        'AL': 'Alabama',
        'AR': 'Arkansas',
        'AS': 'American Samoa',
        'AZ': 'Arizona',
        'CA': 'California',
        'CO': 'Colorado',
        'CT': 'Connecticut',
        'DC': 'District of Columbia',
        'DE': 'Delaware',
        'FL': 'Florida',
        'GA': 'Georgia',
        'GU': 'Guam',
        'HI': 'Hawaii',
        'IA': 'Iowa',
        'ID': 'Idaho',
        'IL': 'Illinois',
        'IN': 'Indiana',
        'KS': 'Kansas',
        'KY': 'Kentucky',
        'LA': 'Louisiana',
        'MA': 'Massachusetts',
        'MD': 'Maryland',
        'ME': 'Maine',
        'MI': 'Michigan',
        'MN': 'Minnesota',
        'MO': 'Missouri',
        'MP': 'Northern Mariana Islands',
        'MS': 'Mississippi',
        'MT': 'Montana',
        'NA': 'National',
        'NC': 'North Carolina',
        'ND': 'North Dakota',
        'NE': 'Nebraska',
        'NH': 'New Hampshire',
        'NJ': 'New Jersey',
        'NM': 'New Mexico',
        'NV': 'Nevada',
        'NY': 'New York',
        'OH': 'Ohio',
        'OK': 'Oklahoma',
        'OR': 'Oregon',
        'PA': 'Pennsylvania',
        'PR': 'Puerto Rico',
        'RI': 'Rhode Island',
        'SC': 'South Carolina',
        'SD': 'South Dakota',
        'TN': 'Tennessee',
        'TX': 'Texas',
        'UT': 'Utah',
        'VA': 'Virginia',
        'VI': 'Virgin Islands',
        'VT': 'Vermont',
        'WA': 'Washington',
        'WI': 'Wisconsin',
        'WV': 'West Virginia',
        'WY': 'Wyoming'
}
```


```python
drug = pd.read_csv("./data/drug16-21.csv").drop(columns = ["DEATHS", "URL"])
drug.columns = map(str.lower, drug.columns)
drug = drug[drug["year"] >= 2016].rename(columns = {"rate": "drug_mortality"})
drug['state'] = drug['state'].replace(abbr_states)
drug.head() #missing DC data
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
      <th>year</th>
      <th>state</th>
      <th>drug_mortality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021</td>
      <td>Alabama</td>
      <td>30.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021</td>
      <td>Alaska</td>
      <td>35.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021</td>
      <td>Arizona</td>
      <td>38.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021</td>
      <td>Arkansas</td>
      <td>22.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021</td>
      <td>California</td>
      <td>26.6</td>
    </tr>
  </tbody>
</table>
</div>



## Binge Drinking 2016-2021

#### Data from:  National Survey on Drug Use and Health


```python
pop = pd.read_excel("./data/popest-annual.xlsm")

def clean_binge(path, year):
    binge = pd.read_excel(path, sheet_name = "Table 14", skiprows = 6)
    binge = binge[["State", "12 or Older\nEstimate"]].rename(columns = {"State": "state",
                                                                       "12 or Older\nEstimate": "estimate num"})
    binge = binge.iloc[5:]
    new = binge.merge(pop[["state", "y"+str(year)]], on="state")
    new["binge_drink_rate"] = (new["estimate num"] / new["y"+str(year)] * 100000).round(2)
    new = new[["state", "binge_drink_rate"]]
    new["year"] = year
    
    return new
    
binge17 = clean_binge("./data/drinking/binge17.xlsx", 2017)
binge18 = clean_binge("./data/drinking/binge18.xlsx", 2018)
binge19 = clean_binge("./data/drinking/binge19.xlsx", 2019)
binge18.head()
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
      <th>state</th>
      <th>binge_drink_rate</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>19.16</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>18.46</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>18.98</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>16.47</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>20.50</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
</div>




```python
binge = pd.read_excel("./data/drinking/binge16.xlsx", sheet_name = "Table 13", skiprows = 6)
binge = binge[["State", "12 or Older\nEstimate"]].rename(columns = {"State": "state",
                                                                   "12 or Older\nEstimate": "estimate num"})
binge = binge.iloc[5:]
binge16 = binge.merge(pop[["state", "y2016"]], on="state")
binge16["binge_drink_rate"] = (binge16["estimate num"] / binge16["y2016"] * 100000).round(2)
binge16 = binge16[["state", "binge_drink_rate"]]
binge16["year"] = 2016

b21 = pd.read_excel("./data/drinking/binge21.xlsx", sheet_name = "Table 15", skiprows = 6)
b21 = b21[["State", "12+\nEstimate"]].rename(columns = {"State": "state",
                                                         "12+\nEstimate": "estimate num"})
b21 = b21.iloc[5:]
binge21 = b21.merge(pop[["state", "y2021"]], on="state")
binge21["binge_drink_rate"] = (binge21["estimate num"] / binge21["y2021"] * 100000).round(2)
binge21 = binge21[["state", "binge_drink_rate"]]
binge21["year"] = 2021
binge21.head()
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
      <th>state</th>
      <th>binge_drink_rate</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>15.83</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>17.06</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>17.39</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>17.02</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>17.34</td>
      <td>2021</td>
    </tr>
  </tbody>
</table>
</div>



### 2020 Data is missing due to COVID-19


```python
binge_list = [binge16, binge17, binge18, binge19, binge21]
binge_drinking = pd.concat(binge_list)
```

## Bachelor Degree Rate among Population 18-24 from 2016-2021

#### Data from: US Census Bureau--Educational Attainment


```python
def attainment_clean(path, year): 
    df1 = pd.read_excel(path, sheet_name = "Data").iloc[:8]

    # filter for rows where the first column is "Bachelor's degree or higher"
    df1 = df1[df1.iloc[:,0] == "Bachelor's degree or higher"]

    # select all columns except the column for Alabama
    df1 = df1.iloc[:, 2:-1]

    # get columns that are bachelor_rate (every 6 columns)
    df1 = df1.iloc[:, list(range(0, df1.shape[1], 6))]

    df2 = pd.melt(df1, var_name='state', value_name='bachelor_rate')
    df2['bachelor_rate'] = df2['bachelor_rate'].apply(lambda x: float(x.replace(',', '').replace('%', '')))
    df2 = df2.iloc[:51]
    df2['state'] = states
    df2["year"] = year
    
    return df2
bachelor21 = attainment_clean('./data/edu21.xlsx', 2021)
bachelor20 = attainment_clean('./data/edu20.xlsx', 2020)
bachelor19 = attainment_clean('./data/edu19.xlsx', 2019)
bachelor18 = attainment_clean('./data/edu18.xlsx', 2018)
bachelor17 = attainment_clean('./data/edu17.xlsx', 2017)
bachelor16 = attainment_clean('./data/edu16.xlsx', 2016)
```


```python
bach_list = [bachelor21, bachelor20, bachelor19, bachelor18, bachelor17, bachelor16]
edu_attainment = pd.concat(bach_list)
```

### Merge past data


```python
df12 = pd.merge(police_correct, teacher_salary, how = "left", on=["state", "year"])
past_data = df12.merge(poverty1520, on=["state", "year"]).merge(offense_rate, on=["state", "year"]).merge(gdp, on=["state", "year"])

```

## Heatmap of US Sexual Offense Rate

### Below is the choropleth of the United State states responding to sexual offense rate. It gives a general idea of which states have higher or lower sexual offense cases and rate.


```python
# Reference:
# https://towardsdatascience.com/simplest-way-of-creating-a-choropleth-map-by-u-s-states-in-python-f359ada7735e
# https://gist.github.com/rogerallen/1583593

# Heat map of sexual offense cases in US in 2021
# The data is collected by FBI, but Florida data is reported by only Seminole and Miccosukee Tribes.
fig = px.choropleth(sexual_factors, 
                    locations='state_codes', 
                    locationmode='USA-states', 
                    scope='usa', 
                    color='sex_rate', 
                    color_continuous_scale='Viridis_r')
fig.show()
```


<div>                            <div id="d5520781-cfba-4ab0-ab90-968dabeb7b3c" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("d5520781-cfba-4ab0-ab90-968dabeb7b3c")) {                    Plotly.newPlot(                        "d5520781-cfba-4ab0-ab90-968dabeb7b3c",                        [{"coloraxis":"coloraxis","geo":"geo","hovertemplate":"state_codes=%{location}<br>sex_rate=%{z}<extra></extra>","locationmode":"USA-states","locations":["AL","AK","AZ","AR","CA","CO","CT","DE","DC","FL","GA","HI","ID","IL","IN","IA","KS","KY","LA","ME","MD","MA","MI","MN","MS","MO","MT","NE","NV","NH","NJ","NM","NY","NC","ND","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VT","VA","WA","WV","WI","WY"],"name":"","z":[33.8,116.2,47.8,101.1,5.2,118.5,51.8,70.3,45.5,null,67.7,44.8,117.5,28.8,70.2,68.8,74.6,70.3,42.0,76.1,27.7,59.1,115.9,87.7,34.0,76.9,152.1,82.2,127.7,128.9,10.5,78.6,8.8,63.2,144.7,67.4,117.0,84.8,11.2,75.1,75.6,129.0,82.7,81.8,153.8,55.9,67.7,75.4,72.4,84.4,95.2],"type":"choropleth"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"geo":{"domain":{"x":[0.0,1.0],"y":[0.0,1.0]},"center":{},"scope":"usa"},"coloraxis":{"colorbar":{"title":{"text":"sex_rate"}},"colorscale":[[0.0,"#fde725"],[0.1111111111111111,"#b5de2b"],[0.2222222222222222,"#6ece58"],[0.3333333333333333,"#35b779"],[0.4444444444444444,"#1f9e89"],[0.5555555555555556,"#26828e"],[0.6666666666666666,"#31688e"],[0.7777777777777778,"#3e4989"],[0.8888888888888888,"#482878"],[1.0,"#440154"]]},"legend":{"tracegroupgap":0},"margin":{"t":60}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('d5520781-cfba-4ab0-ab90-968dabeb7b3c');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
#contrast with school district safety score (score.y)

fig1 = px.choropleth(sexual_factors, 
                    locations='state_codes', 
                    locationmode='USA-states', 
                    scope='usa', 
                    color='score.y', 
                    color_continuous_scale='Viridis_r')
fig1.show()
```


<div>                            <div id="4be43526-e007-47e3-9173-0af280fcebcf" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("4be43526-e007-47e3-9173-0af280fcebcf")) {                    Plotly.newPlot(                        "4be43526-e007-47e3-9173-0af280fcebcf",                        [{"coloraxis":"coloraxis","geo":"geo","hovertemplate":"state_codes=%{location}<br>score.y=%{z}<extra></extra>","locationmode":"USA-states","locations":["AL","AK","AZ","AR","CA","CO","CT","DE","DC","FL","GA","HI","ID","IL","IN","IA","KS","KY","LA","ME","MD","MA","MI","MN","MS","MO","MT","NE","NV","NH","NJ","NM","NY","NC","ND","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VT","VA","WA","WV","WI","WY"],"name":"","z":[26.5,81.4,84.4,21.2,91.6,55.5,33.3,74.1,86.4,100.0,42.4,20.6,68.3,47.0,11.0,3.8,39.8,0.0,33.0,9.5,91.9,23.1,38.1,41.5,16.7,32.7,55.1,42.0,98.6,42.0,59.1,74.5,62.3,50.0,25.5,32.6,39.1,52.5,50.2,63.7,13.0,34.2,44.2,44.7,20.3,23.3,65.9,30.5,25.5,51.5,49.0],"type":"choropleth"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"geo":{"domain":{"x":[0.0,1.0],"y":[0.0,1.0]},"center":{},"scope":"usa"},"coloraxis":{"colorbar":{"title":{"text":"score.y"}},"colorscale":[[0.0,"#fde725"],[0.1111111111111111,"#b5de2b"],[0.2222222222222222,"#6ece58"],[0.3333333333333333,"#35b779"],[0.4444444444444444,"#1f9e89"],[0.5555555555555556,"#26828e"],[0.6666666666666666,"#31688e"],[0.7777777777777778,"#3e4989"],[0.8888888888888888,"#482878"],[1.0,"#440154"]]},"legend":{"tracegroupgap":0},"margin":{"t":60}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('4be43526-e007-47e3-9173-0af280fcebcf');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Victims Profile

### Getting closer to the victims of sexual offense, who are they? The age and sex distribution are included in order to find the out who is more likely be targeted.

### Victims Sex Distribution


```python
sex = pd.read_excel("./data/victim_sex.xlsx", skiprows = 5)
sex.columns = ["offense category", "total", "male", "female", "unknown sex"]
sex = sex[sex["offense category"]=="Sex Offenses"].transpose()
sex.columns = ["victims"]

#Bar plot of victims by sex
sex.iloc[2:].plot.bar(legend=None, figsize=(10, 7))
plt.xlabel("sex", fontsize = 14)
plt.ylabel("number of victims", fontsize = 14)
plt.title("Sex Offense Victims by Sex", fontsize = 16)
plt.xticks(rotation = 0)

#Pie graph of victims' sex distribution
ax = sex.iloc[2:].plot.pie(y="victims", figsize=(10,10), autopct='%1.1f%%')
plt.legend(loc="upper left")
plt.title("Sexual Offense Victims Proportion by Sex", fontsize = 16)
ax.set_ylabel('')
```




    Text(0, 0.5, '')




    
![png](output_59_1.png)
    



    
![png](output_59_2.png)
    


### It's evident that victims are overwhelmingly females, made up 87.4% of all victims. Now we know the sex distribution. What about age? Are the young ladies more likely to be the target? Or the immatured, vulnerable underaged girls? 

### Victim Age Distribution


```python
age = pd.read_excel("./data/victim_age.xlsx", skiprows = 5)
age = age[age["Offense Category"]=="Sex Offenses"]
age = age.drop(columns=["Unnamed: 1"]).transpose().iloc[1:]
age.reset_index(inplace=True)
age.columns = ["age", "number"]

# age.plot.bar(legend=None)
plt.figure(figsize=(10,7))
sns.histplot(x=age["age"], weights=age["number"], discrete=True,
             color='darkblue', edgecolor='black', kde=True)
plt.xticks(rotation = 90)
plt.xlabel("age", fontsize = 14)
plt.ylabel("number of victims", fontsize = 14)
plt.title("Sexual Offense Victims by Age", fontsize = 16)
plt.show()
```


    
![png](output_62_0.png)
    


### 5 years for each age category. Left skewed curve shows that majority of the victims are in young ages, mostly between 11-15 who are vulnerable underaged teen and even children.

## Offender Profile

### After the victims proflie, who are the offenders? What's their sex distribution, age distribution? Will these two parameters have the same pattern for victims and offenders? Besides sex and age, relationship with the victim and the offender drug+alcohol use are also included. Do the offenders know the victims? Or they are total strangers. For alcohol is always used as an justification for wrong doing, are the offenders sober when they commit crime?

### Offender Sex Distribution vs Victims Sex Distribution


```python
offend_sex = pd.read_excel("./data/offender_sex.xlsx", skiprows = 5)
offend_sex.columns = ["offense category", "total", "male", "female", "unknown sex"]

offend_sex = offend_sex[offend_sex["offense category"]=="Sex Offenses"].transpose().iloc[1:]
offend_sex.columns = ["offenders"]
sex_both = pd.concat([sex, offend_sex], axis =1).iloc[1:].drop(["total"])
```


```python
sex_both.plot.bar(figsize=(12,8))
plt.xlabel("sex", fontsize = 14)
plt.ylabel("number of victims/offenders", fontsize = 14)
plt.title("Sexual Offense Victims vs. Offenders by Sex", fontsize = 16)

# delete total
```




    Text(0.5, 1.0, 'Sexual Offense Victims vs. Offenders by Sex')




    
![png](output_67_1.png)
    


### The result shows a opposite trend. The female victims outnumber the male victims while male offenders outnumber the female victims.

### Offender Age Distribution


```python
offend_age = pd.read_excel("./data/offender_age.xlsx", skiprows = 5)

offend_age = offend_age[offend_age["Offense Category"]=="Sex Offenses"]
offend_age = offend_age.drop(columns=["Unnamed: 1"]).transpose().iloc[1:]
offend_age.columns = ["number"]
offend_age.plot.bar(legend=None, color = "orange", figsize=(12,8))
plt.xlabel("age", fontsize = 14)
plt.ylabel("number of offenders", fontsize = 14)
plt.title("Sexual Offense Offenders by Age", fontsize = 16)
```




    Text(0.5, 1.0, 'Sexual Offense Offenders by Age')




    
![png](output_70_1.png)
    


### Offender age distribution is also left skewed but smoother. Most of the offenders are teenagers and physically capable adults who are between 11-35.

### Relationship with the Victim


```python
relation = pd.read_excel("./data/relationship.xlsx", skiprows = 4)
relation.columns = ["offense category", "total", "family member", "family member and other",
                   "known to victims and other", "stranger", "all other"]
relation = relation[relation["offense category"]=="Sex Offenses"]
relation["family member and other"] = relation["family member"] + relation["family member and other"]

relation = relation.drop(columns = ["total", "family member"]).transpose().iloc[1:]
relation.columns = ["number"]
ax = relation.plot.pie(y="number", figsize=(12, 8), autopct='%1.1f%%')
plt.legend(loc="upper left")
plt.xlabel("relationships", fontsize = 14)
plt.title("Offenders' Relationship With Victims Proportion", fontsize = 16)
ax.set_ylabel('')
```




    Text(0, 0.5, '')




    
![png](output_73_1.png)
    


### "Family member and other" category consists victims related to all offenders and victims related to at least one offenders. They together made up 80.5% of the cases, indicating most of the offenses are done by acquaintance. 

### Offender Use of Drug or Alcohol


```python
sober = pd.read_excel("./data/drug_alco.xlsx", skiprows = 5)
sober.columns = ["offense category", "total", "drugs", "percentage_drug",
                "alcohol", "percentage_alco"]
sober = sober[sober["offense category"]=="Sex Offenses"].drop(columns = ["percentage_drug", "percentage_alco"])
sober["no use"] = sober["total"] - sober["drugs"] - sober["alcohol"]
sober = sober.drop(columns = ["total"]).transpose().iloc[1:]
sober.columns = ["number"]

ax = sober.plot.pie(y="number", figsize=(12, 8), autopct='%1.1f%%', label = None)
plt.legend(loc="upper left")
plt.xlabel("use of drug or alcohol", fontsize = 14)
plt.title("Proportion of Offenders Use Alcohol or Drug in Sexual Offenses", fontsize = 16)
ax.set_ylabel('')
```




    Text(0, 0.5, '')




    
![png](output_76_1.png)
    


### Nearly 90% of the sexual offense is commited when offenders are sober, free from any use of alcohol or drugs.


```python
# All the data used in previous analysis are from FBI Crime Data Explorer
```

# State-level demographical features

### The purpose of the project is to find out whether the demographic features will affect sexual offense rate. What are those features? I have decided to include 8 features: police spending, primary and secondary school teacher average salary, poverty rate, GDP per capita, education spending per pupil, drug overdose mortality rate, binge drinking rate, and bachelor degree attainment rate.


```python
# data adding and cleaning

sexual_factors.sort_values("sex_rate", ascending = False).head()
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
      <th>state</th>
      <th>reported_rape_cases</th>
      <th>rape_arrests</th>
      <th>sex_offense_arrests</th>
      <th>salary</th>
      <th>score.x</th>
      <th>score.y</th>
      <th>policing_correction_spend_per_capita</th>
      <th>policing_correction_expenditure</th>
      <th>correction_spending_per</th>
      <th>state_codes</th>
      <th>sex offenses</th>
      <th>population</th>
      <th>sex_rate</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>44</th>
      <td>Utah</td>
      <td>5134</td>
      <td>291</td>
      <td>498</td>
      <td>54678</td>
      <td>55.53</td>
      <td>20.3</td>
      <td>484</td>
      <td>1553</td>
      <td>68%</td>
      <td>UT</td>
      <td>5134.0</td>
      <td>3337975.0</td>
      <td>153.8</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Montana</td>
      <td>1680</td>
      <td>62</td>
      <td>118</td>
      <td>52135</td>
      <td>51.55</td>
      <td>55.1</td>
      <td>608</td>
      <td>649</td>
      <td>74%</td>
      <td>MT</td>
      <td>1680.0</td>
      <td>1104271.0</td>
      <td>152.1</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>34</th>
      <td>North Dakota</td>
      <td>1121</td>
      <td>51</td>
      <td>81</td>
      <td>53525</td>
      <td>58.42</td>
      <td>25.5</td>
      <td>574</td>
      <td>437</td>
      <td>70%</td>
      <td>ND</td>
      <td>1121.0</td>
      <td>774948.0</td>
      <td>144.7</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>41</th>
      <td>South Dakota</td>
      <td>1155</td>
      <td>36</td>
      <td>51</td>
      <td>48984</td>
      <td>51.19</td>
      <td>34.2</td>
      <td>478</td>
      <td>423</td>
      <td>85%</td>
      <td>SD</td>
      <td>1155.0</td>
      <td>895376.0</td>
      <td>129.0</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>29</th>
      <td>New Hampshire</td>
      <td>1790</td>
      <td>71</td>
      <td>100</td>
      <td>59622</td>
      <td>59.19</td>
      <td>42.0</td>
      <td>525</td>
      <td>714</td>
      <td>45%</td>
      <td>NH</td>
      <td>1790.0</td>
      <td>1388992.0</td>
      <td>128.9</td>
      <td>2021</td>
    </tr>
  </tbody>
</table>
</div>




```python
poverty_state = pd.read_excel("./data/poverty_state.xlsx").drop(columns=["Unnamed: 2"]).rename(columns={"below_poverty":
                                                                                                       "poverty_rate"})

sex_offense = sexual_factors.merge(poverty_state, how="left", on="state").merge(gdp21, how="left", on = ["state", "year"])

# original data selection
selected = sex_offense[["state",
                        "salary",
                        "policing_correction_spend_per_capita",
                        "poverty_rate",
                        "sex_rate","year",
                        "gdp_capita"]]

```


```python
all_list = [past_data, selected]
sex_offense_new = pd.concat(all_list)
sex_offense_new = sex_offense_new[(sex_offense_new["year"]>2015) & (sex_offense_new["year"]<2020)]
model_data = sex_offense_new.merge(edu_spending, 
                                   on=["year", "state"], 
                                   how = "left").merge(drug,
                                                       on=["year", "state"], 
                                                       how = "left").merge(binge_drinking,
                                                                           on=["state", "year"],
                                                                           how = "left").merge(edu_attainment,
                                                                                               on=["state", "year"], 
                                                                                               how = "left")
model_data = model_data.drop(columns=["state", "year"]).dropna()
model_data.head()
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
      <th>policing_correction_spend_per_capita</th>
      <th>salary</th>
      <th>poverty_rate</th>
      <th>sex_rate</th>
      <th>gdp_capita</th>
      <th>edu_spending_per_pupil</th>
      <th>drug_mortality</th>
      <th>binge_drink_rate</th>
      <th>bachelor_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>405.28</td>
      <td>49781.0</td>
      <td>17.2</td>
      <td>39.4</td>
      <td>37158.0</td>
      <td>9242.677695</td>
      <td>16.2</td>
      <td>17.69</td>
      <td>7.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>954.48</td>
      <td>67443.0</td>
      <td>9.9</td>
      <td>141.9</td>
      <td>63304.0</td>
      <td>17509.975316</td>
      <td>16.8</td>
      <td>19.53</td>
      <td>7.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>569.62</td>
      <td>45477.0</td>
      <td>16.4</td>
      <td>47.5</td>
      <td>38940.0</td>
      <td>7613.006435</td>
      <td>20.3</td>
      <td>18.50</td>
      <td>7.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>396.26</td>
      <td>48220.0</td>
      <td>17.2</td>
      <td>71.7</td>
      <td>36502.0</td>
      <td>9845.568548</td>
      <td>14.0</td>
      <td>16.71</td>
      <td>7.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>843.21</td>
      <td>72842.0</td>
      <td>14.4</td>
      <td>34.9</td>
      <td>58974.0</td>
      <td>11495.363449</td>
      <td>11.2</td>
      <td>19.98</td>
      <td>10.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
X=model_data.drop(columns = ["sex_rate"])
y=model_data["sex_rate"]
```


```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report
from sklearn.metrics import r2_score
from rfpimp import permutation_importances

X_train, X_test, ytrain, ytest = train_test_split(X, y, test_size=0.3, random_state=42)
```


```python
rf = RandomForestRegressor(max_depth=2, random_state=0, oob_score = True)
rf.fit(X_train, ytrain)
```




    RandomForestRegressor(max_depth=2, oob_score=True, random_state=0)




```python
y_pred_test = rf.predict(X_test)
```


```python
print('R^2 Training Score: {:.2f} \nOOB Score: {:.2f} \nR^2 Validation Score: {:.2f}'.format(rf.score(X_train, ytrain), 
                                                                                             rf.oob_score_,
                                                                                             rf.score(X_test, ytest)))

```

    R^2 Training Score: 0.69 
    OOB Score: 0.52 
    R^2 Validation Score: 0.08



```python
def r2(rf, X_train, ytrain):
    return r2_score(ytrain, rf.predict(X_train))

perm_imp_rfpimp = permutation_importances(rf, X_train, ytrain, r2)
perm_imp_rfpimp # bar plot
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
      <th>Importance</th>
    </tr>
    <tr>
      <th>Feature</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>policing_correction_spend_per_capita</th>
      <td>1.159549</td>
    </tr>
    <tr>
      <th>edu_spending_per_pupil</th>
      <td>0.052886</td>
    </tr>
    <tr>
      <th>salary</th>
      <td>0.052280</td>
    </tr>
    <tr>
      <th>drug_mortality</th>
      <td>0.009004</td>
    </tr>
    <tr>
      <th>bachelor_rate</th>
      <td>0.004535</td>
    </tr>
    <tr>
      <th>gdp_capita</th>
      <td>0.001573</td>
    </tr>
    <tr>
      <th>binge_drink_rate</th>
      <td>0.000302</td>
    </tr>
    <tr>
      <th>poverty_rate</th>
      <td>-0.000240</td>
    </tr>
  </tbody>
</table>
</div>




```python
perm_imp_rfpimp.plot.barh(y='Importance', rot=0)
```




    <AxesSubplot:ylabel='Feature'>




    
![png](output_89_1.png)
    


### Below is the stacked bar plot, which aim to observe a relationship between the features and the sexual offense rate. The green bar indicates the police spending, orange indicates the sexual offense rate, and blue is the average school district score.


```python
#plt.figure(figsize=(24, 20))
sub_full = sexual_factors[["sex_rate","score.x","policing_correction_spend_per_capita"]]
sub_full.plot(kind='bar', stacked=True, figsize = (16, 14))
plt.title("Comparison of Sexual Assult Rate, School District Score, and Police Spending of Each State, 2021")
plt.xticks(rotation = 90)
plt.legend(loc='best', labels = ["sexual assault rate", "school district score", "police correction spending per capita"])
plt.xlabel("state", fontsize = 12)
plt.ylabel("score/rate_per_100,000/police spending", fontsize = 12)
```




    Text(0, 0.5, 'score/rate_per_100,000/police spending')




    
![png](output_91_1.png)
    


### For the states that have high sexual offense rate, their average school district score is lower(shorter stick), and vice versa. The relationship between sexual offense rate and police spending is not obvious. 

### According to U.S. News best States Ranking of Education, New Jersey ranks in NO.1 for Pre-K to 12, and No.27 for higher education; Alaska ranks in NO.49 for Pre-K to 12, NO.36 for higher education.

#### Note: Poverty rate is not included in the bar plot since it does not have an evident pattern, and the number variance is too small to be compared visually.
