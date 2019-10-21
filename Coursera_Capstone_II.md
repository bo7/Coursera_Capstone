
<h3>Add libraries</h3>




```python
import pandas as pd
import requests
from bs4 import BeautifulSoup

```

<h3>Read table from Wikipedia in pandas data frame</h3>


```python
url = "https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M"
res = requests.get(url)
soup = BeautifulSoup(res.content,'lxml')
table = soup.find_all('table')[0] 
df = pd.read_html(str(table))[0]
```

<h3>Clean and amend data frame</h3>
<ol>
  <li>remove not assigned boroughs</li>
  <li>join postcodes with multiple boroughs in one comma seperated row</li>
  <li>copy not assigned neighbourhoods to borough</li>
</ol>


```python
# Clean Not assigned Boroughs  
df = df[~df['Borough'].isin(['Not assigned'])]
# Join grouped Boroughs to one comma seperated line
df =df.groupby(['Postcode','Borough'],as_index=False, sort=False).agg(','.join)
# Boroughs with no neighbourhood get the borough itself as neighbourhood
df.Neighbourhood = df.Borough.where(df.Neighbourhood == 'Not assigned', df.Neighbourhood)
```

<h3>Print head of data frame and shape</h3>


```python
print(df.head())
print(df.shape)
```

      Postcode           Borough                    Neighbourhood
    0      M3A        North York                        Parkwoods
    1      M4A        North York                 Victoria Village
    2      M5A  Downtown Toronto         Harbourfront,Regent Park
    3      M6A        North York  Lawrence Heights,Lawrence Manor
    4      M7A      Queen's Park                     Queen's Park
    (103, 3)



```python
''
```
