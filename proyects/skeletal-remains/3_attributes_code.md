# Dataset

El dataset se encuentra en un archivo `.accdb` (Microsoft Access) asi que lo primero que vamos a hacer es exportarlo como `.csv`

![Acces DB](./img/accesdb.png)

Analizándolo rápidamente en un editor de texto podemos ver que contiene caracteres que no van a poder ser decodeados con UTF-8 (`�`) ya que los comentarios estan en danes. Los remplazamos rápidamente por `?`.

![UTF-8 Error](./img/utf8Error.png)

Ahora ya podemos cargarlo con `pandas`

[Vista detallada del dataset ➡](./docs/pf_overview.html)

### Importando librerias

```python
import missingno as msno
import pandas as pd
import pandas_profiling as pf
import numpy as np
import matplotlib.pyplot as plt
import matplotlib
import seaborn as sns
matplotlib.rcParams['figure.figsize'] = [15, 10]
```

### Cargando el dataset

```python
data = pd.read_csv('dataindsamling.csv')
```


```python
data.head()
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
      <th>ID</th>
      <th>Location</th>
      <th>Site_Number</th>
      <th>Age_Minumum</th>
      <th>Age_Maximum</th>
      <th>Sex</th>
      <th>Grave Number</th>
      <th>Canine number</th>
      <th>Canine largest age</th>
      <th>Canine 2nd largest age</th>
      <th>...</th>
      <th>Height in grave</th>
      <th>Abnormalities Vertebras</th>
      <th>Femur left</th>
      <th>Femur right</th>
      <th>Abnormalities Femur</th>
      <th>Notes</th>
      <th>Date</th>
      <th>Signature</th>
      <th>Hyperplasia</th>
      <th>Teeth Scorable</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>20</td>
      <td>24.0</td>
      <td>Male</td>
      <td>G40</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>173.5</td>
      <td>NaN</td>
      <td>49.6</td>
      <td>50.0</td>
      <td>Br?kket Postmortalt</td>
      <td>NaN</td>
      <td>5/5/2008</td>
      <td>Mwod</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>35</td>
      <td>45.0</td>
      <td>Male</td>
      <td>G312</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4.5</td>
      <td>...</td>
      <td>170.0</td>
      <td>NaN</td>
      <td>48.4</td>
      <td>48.5</td>
      <td>NaN</td>
      <td>l?bedannelser</td>
      <td>5/8/2008</td>
      <td>MWOD</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>50</td>
      <td>60.0</td>
      <td>Male</td>
      <td>G229</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>171.5</td>
      <td>To hvirvler fusioneret - har ingen betydning f...</td>
      <td>50.8</td>
      <td>51.3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5/8/2008</td>
      <td>MWOD</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>30</td>
      <td>40.0</td>
      <td>Male</td>
      <td>G257</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>165.0</td>
      <td>NaN</td>
      <td>45.4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Male?, h?jre femur kan ikke m?les da nedbrudt,...</td>
      <td>5/8/2008</td>
      <td>MWOD</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>45</td>
      <td>55.0</td>
      <td>Male</td>
      <td>G74</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>165.0</td>
      <td>NaN</td>
      <td>47.5</td>
      <td>46.6</td>
      <td>NaN</td>
      <td>kraniet mangler - kig efter om det er p? udsti...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>

