
# Data Preparation

Importando librerias


```python
import pandas as pd
from datetime import datetime
```


```python
raw_bus_data = pd.read_csv('../data/buses.csv')
```


```python
raw_bus_data.head()
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
      <th>id</th>
      <th>type</th>
      <th>busCode</th>
      <th>line</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>76</td>
      <td>Bus</td>
      <td>76</td>
      <td>217</td>
      <td>-56.222400</td>
      <td>-34.855885</td>
      <td>2018-10-02T14:38:53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>984</td>
      <td>Bus</td>
      <td>984</td>
      <td>540</td>
      <td>-56.201860</td>
      <td>-34.909360</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>288</td>
      <td>Bus</td>
      <td>288</td>
      <td>7898</td>
      <td>-56.175415</td>
      <td>-34.901110</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1111</td>
      <td>Bus</td>
      <td>1111</td>
      <td>254</td>
      <td>-56.136640</td>
      <td>-34.872833</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Bus</td>
      <td>22</td>
      <td>540</td>
      <td>-56.135277</td>
      <td>-34.845554</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
  </tbody>
</table>
</div>



El type es retornado por la plataforma Orion y no aporta valor a la solucion


```python
raw_bus_data = raw_bus_data.drop(['type'], axis=1)
raw_bus_data.head()
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
      <th>id</th>
      <th>busCode</th>
      <th>line</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>76</td>
      <td>76</td>
      <td>217</td>
      <td>-56.222400</td>
      <td>-34.855885</td>
      <td>2018-10-02T14:38:53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>984</td>
      <td>984</td>
      <td>540</td>
      <td>-56.201860</td>
      <td>-34.909360</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>288</td>
      <td>288</td>
      <td>7898</td>
      <td>-56.175415</td>
      <td>-34.901110</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1111</td>
      <td>1111</td>
      <td>254</td>
      <td>-56.136640</td>
      <td>-34.872833</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>22</td>
      <td>540</td>
      <td>-56.135277</td>
      <td>-34.845554</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
  </tbody>
</table>
</div>



Podemos ver que id y busCode son iguales, ahora intentamos demostrarlo comparando el largo del dataset con el largo del subdataset en el cual id y busCuode son iguales


```python
print(len(raw_bus_data)- len(raw_bus_data[raw_bus_data.id == raw_bus_data.busCode]))
```

    0


Podemos ver que son iguales y eliminamos busCode


```python
raw_bus_data = raw_bus_data.drop(['busCode'], axis=1)
raw_bus_data.head()
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
      <th>id</th>
      <th>line</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>76</td>
      <td>217</td>
      <td>-56.222400</td>
      <td>-34.855885</td>
      <td>2018-10-02T14:38:53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>984</td>
      <td>540</td>
      <td>-56.201860</td>
      <td>-34.909360</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>288</td>
      <td>7898</td>
      <td>-56.175415</td>
      <td>-34.901110</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1111</td>
      <td>254</td>
      <td>-56.136640</td>
      <td>-34.872833</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>540</td>
      <td>-56.135277</td>
      <td>-34.845554</td>
      <td>2018-10-02T14:39:08</td>
    </tr>
  </tbody>
</table>
</div>



Podemos ver que el timestamp esta en encoding ISO 8601, lo pasamos a un formato de date que puedamos trabajar con python


```python
raw_bus_data.timestamp = raw_bus_data.timestamp.apply(lambda t: datetime.strptime(t,'%Y-%m-%dT%H:%M:%S'))
raw_bus_data.head()
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
      <th>id</th>
      <th>line</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>76</td>
      <td>217</td>
      <td>-56.222400</td>
      <td>-34.855885</td>
      <td>2018-10-02 14:38:53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>984</td>
      <td>540</td>
      <td>-56.201860</td>
      <td>-34.909360</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>288</td>
      <td>7898</td>
      <td>-56.175415</td>
      <td>-34.901110</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1111</td>
      <td>254</td>
      <td>-56.136640</td>
      <td>-34.872833</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>540</td>
      <td>-56.135277</td>
      <td>-34.845554</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
  </tbody>
</table>
</div>



Vamos a guardar los datos con los cambios realizados en `bus_data_stage_1.csv`


```python
raw_bus_data.to_csv('../data/bus_data_stage_1.csv')
```
