
# Outliers


```python
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
import matplotlib.pylab as pl
import numpy as np
from datetime import datetime
```

Al guardar el dataset en la sección anterior, pandas agrega una columna `Unnamed: 0` con los indices de las filas, esta columna no nos es util por lo cual la eliminamos al cargar el dataset


```python
stage_1_data = pd.read_csv('../data/bus_data_stage_1.csv')
stage_1_data = stage_1_data.drop(['Unnamed: 0'], axis=1)
stage_1_data.head()
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



Vemos los diferentes tipos de datos con los que estamos trabajando


```python
stage_1_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 96496 entries, 0 to 96495
    Data columns (total 5 columns):
    id           96496 non-null int64
    line         96496 non-null int64
    longitude    96496 non-null float64
    latitude     96496 non-null float64
    timestamp    96496 non-null object
    dtypes: float64(2), int64(2), object(1)
    memory usage: 3.7+ MB


Reordenamos el dataset y vemos algunas medidas estadisticas de los datos


```python
_ = pd.DataFrame(columns=['id', 'line', 'longitude', 'latitude', 'timestamp'])
_.id   = stage_1_data.id
_.line = stage_1_data.line
_.longitude  = stage_1_data.longitude
_.latitude   = stage_1_data.latitude
_.timestamp  = stage_1_data.timestamp
stage_1_data = pd.DataFrame(_)
del _
```


```python
stage_1_data.describe()
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>96496.000000</td>
      <td>96496.000000</td>
      <td>96496.000000</td>
      <td>96496.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>348.944153</td>
      <td>2671.002933</td>
      <td>-56.147596</td>
      <td>-34.875496</td>
    </tr>
    <tr>
      <th>std</th>
      <td>368.219358</td>
      <td>3113.855989</td>
      <td>0.038840</td>
      <td>0.029654</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2.000000</td>
      <td>217.000000</td>
      <td>-56.256016</td>
      <td>-34.928585</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>53.000000</td>
      <td>266.000000</td>
      <td>-56.174942</td>
      <td>-34.900887</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>110.000000</td>
      <td>565.000000</td>
      <td>-56.148083</td>
      <td>-34.881416</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>709.000000</td>
      <td>7516.000000</td>
      <td>-56.128693</td>
      <td>-34.863860</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1111.000000</td>
      <td>7929.000000</td>
      <td>-55.995388</td>
      <td>-34.715830</td>
    </tr>
  </tbody>
</table>
</div>



![Montevideo](../images/montevideo.png)

Sabemos que montevideo esta contenido en una _Bounding Box_ con las siguientes coordenadas:

Izquierda: **-56.434944**

Derecha  : **-56.015140**

Arriba   : **-34.696519**

Abajo    : **-34.938120**


```python
UPPER_BOUND, LOWER_BOUND, RIGHT_BOUND, LEFT_BOUND = [-34.696519, -34.938120, -56.015140,  -56.434944]
UPPER_LAT = stage_1_data.latitude.max()
LOWER_LAT = stage_1_data.latitude.min()
RIGHT_LON = stage_1_data.longitude.max()
LEFT_LON  = stage_1_data.longitude.min()
print('Max Latitude: {0}\nMin Latitude: {1}\nMax Longitude: {2}\nMin Longitude: {3}\n'
      .format(UPPER_LAT, LOWER_LAT, RIGHT_LON, LEFT_LON))
print('Upper boundin is OK') if UPPER_LAT <= UPPER_BOUND else print('Upper boundin is WRONG with difference: {0}'
                                                                         .format(abs(UPPER_BOUND-UPPER_LAT)))
print('Lower boundin is OK') if LOWER_LAT >= LOWER_BOUND else print('Lower boundin is WRONG with difference: {0}'
                                                                         .format(abs(LOWER_BOUND-LOWER_LAT)))
print('Right boundin is OK') if RIGHT_LON <= RIGHT_BOUND else print('Right boundin is WRONG with difference: {0}'
                                                                         .format(abs(RIGHT_BOUND-RIGHT_LON)))
print('Left boundin is OK') if LEFT_LON >= LEFT_BOUND else print('Left  boundin is WRONG with difference: {0}'
                                                                         .format(abs(LEFT_BOUND-LEFT_LON)))
```

    Max Latitude: -34.71583
    Min Latitude: -34.928585
    Max Longitude: -55.995388
    Min Longitude: -56.256016
    
    Upper boundin is OK
    Lower boundin is OK
    Right boundin is WRONG with difference: 0.01975200000000399
    Left boundin is OK


Podemos ver que tenemos coordenadas que se salen de la bounding box de Monteivode, esto puede ser por algunas lieas que comienzan o terminan fuera de Montevideo. Ahora vamos a ver cuales son las cordenadas que estan por fuera.


```python
right_bound_violations = stage_1_data[stage_1_data.longitude >= RIGHT_BOUND]
right_bound_violations.describe()
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>134.000000</td>
      <td>134.000000</td>
      <td>134.000000</td>
      <td>134.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>416.820896</td>
      <td>4853.567164</td>
      <td>-56.004511</td>
      <td>-34.864850</td>
    </tr>
    <tr>
      <th>std</th>
      <td>203.151962</td>
      <td>3499.197086</td>
      <td>0.005954</td>
      <td>0.003143</td>
    </tr>
    <tr>
      <th>min</th>
      <td>249.000000</td>
      <td>863.000000</td>
      <td>-56.015137</td>
      <td>-34.870777</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>252.000000</td>
      <td>863.000000</td>
      <td>-56.009283</td>
      <td>-34.867215</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>267.000000</td>
      <td>7899.000000</td>
      <td>-56.003277</td>
      <td>-34.864445</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>545.000000</td>
      <td>7899.000000</td>
      <td>-55.999534</td>
      <td>-34.862208</td>
    </tr>
    <tr>
      <th>max</th>
      <td>756.000000</td>
      <td>7899.000000</td>
      <td>-55.995388</td>
      <td>-34.859890</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.plot(right_bound_violations.longitude,
         right_bound_violations.latitude)
```




    [<matplotlib.lines.Line2D at 0x11004b630>]




![png](output_14_1.png)


Podemos ver que es la linea 142 que llega hasta la terminal ANCAP de la ciudad de la costa por lo cual no es un outlier.

![Montevideo](../images/right_bound_error.png)

![Montevideo](../images/right_bound_error_2.png)

Ahora que sabemos que no existen outliers fuera de montevideo revisaremos las lineas de forma independiente

## Outliers por linea

Primero tenemos que agrupar por linea los datos


```python
data_sorted_by_line = stage_1_data.sort_values(by='line')
lines = data_sorted_by_line.line.unique()
data_sorted_by_line.head()
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
      <th>45991</th>
      <td>13</td>
      <td>217</td>
      <td>-56.116450</td>
      <td>-34.871450</td>
      <td>2018-10-02 16:06:25</td>
    </tr>
    <tr>
      <th>45998</th>
      <td>99</td>
      <td>217</td>
      <td>-56.155420</td>
      <td>-34.883700</td>
      <td>2018-10-02 16:06:25</td>
    </tr>
    <tr>
      <th>46008</th>
      <td>29</td>
      <td>217</td>
      <td>-56.222800</td>
      <td>-34.855732</td>
      <td>2018-10-02 16:06:25</td>
    </tr>
    <tr>
      <th>46017</th>
      <td>105</td>
      <td>217</td>
      <td>-56.083332</td>
      <td>-34.882200</td>
      <td>2018-10-02 16:06:25</td>
    </tr>
  </tbody>
</table>
</div>




```python
line_numbers = data_sorted_by_line.line.unique()
print('Lines are:')
line_numbers
```

    Lines are:





    array([ 217,  218,  227,  231,  236,  237,  242,  254,  266,  272,  340,
            341,  342,  343,  344,  345,  372,  386,  389,  395,  400,  403,
            498,  501,  522,  540,  542,  565,  566,  863,  873, 1476, 1483,
           1922, 2388, 2390, 2391, 2392, 2393, 2402, 2403, 2408, 2409, 2422,
           2454, 2601, 2775, 2924, 7512, 7516, 7517, 7526, 7528, 7529, 7533,
           7703, 7704, 7898, 7899, 7902, 7903, 7918, 7919, 7920, 7921, 7926,
           7927, 7928, 7929])




```python
lines = {}

for line_number in line_numbers:
    lines[line_number] = stage_1_data[stage_1_data.line==line_number]
```

Ahora tenemos un diccionaro de la forma { **linea** : _informacionde-la-linea_ }

Vamos a crear algunas funciones para facilitar la busqueda de outliers.


```python
def plot_line(line):
    plt.figure(figsize=(10,5))
    plt.title('Line {0}'.format(line.line.iloc[0]))
    plt.plot(line.longitude, line.latitude,'-o')
    plt.show()

def plot_ids_line(line):
    line_ids = line.id.unique()
    plt.figure(figsize=(10,5))
    plt.title(line.line.iloc[0])
    different_ids = len(line_ids)
    colors = pl.cm.jet(np.linspace(0,1,different_ids))
    for size, id_ in enumerate(line_ids):
        l = line[line.id == id_]
        plt.plot(l.longitude, l.latitude, '-o', color=colors[size],markersize=(different_ids-size)*3)
    plt.show()
    
def plot_line_speed(line):
    line_ids = line.id.unique()
    plt.figure(figsize=(17,6))
    plt.title(line.line.iloc[0])
    line.drop(line.index[-1], inplace=True)
    max_speed = int(line.speed.max())
    min_speed = int(line.speed.min())
    speeds = max_speed - min_speed 
    colors = pl.cm.jet(np.linspace(0, 1, speeds + 1))
    sps = []
    for step in line.itertuples():
        plt.plot(step.longitude, step.latitude, '-o', color=colors[int(step.speed)],markersize=step.speed)
        sps.append(str(int(ms_to_kh(step.speed)))+' k/h')
    plt.legend(sps[:20])
    plt.legend(sps[:20])
    plt.show()
    
def ms_to_kh(ms):
    return (ms*18)/5
```

Ahora vamos a mostrar graficamente los reorridos
las prineras 10 lineas.


```python
for line_number in line_numbers[:10]:
    plot_line(lines[line_number])
```


![png](output_23_0.png)



![png](output_23_1.png)



![png](output_23_2.png)



![png](output_23_3.png)



![png](output_23_4.png)



![png](output_23_5.png)



![png](output_23_6.png)



![png](output_23_7.png)



![png](output_23_8.png)



![png](output_23_9.png)


Podemos ver que tenemos un problema con la mayoria de las lineas, vamos a analizarlo tomando la linea 1476.


```python
line_1476 = lines[1476] 
plot_line(line_1476)
```


![png](output_25_0.png)


Vamos a ordenarlos por tiempo


```python
line_1476 = line_1476.sort_values(by='timestamp')
line_1476.head()
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
      <th>10</th>
      <td>285</td>
      <td>1476</td>
      <td>-56.169277</td>
      <td>-34.903220</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>60</th>
      <td>263</td>
      <td>1476</td>
      <td>-56.082610</td>
      <td>-34.884945</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>110</th>
      <td>525</td>
      <td>1476</td>
      <td>-56.080530</td>
      <td>-34.892750</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>155</th>
      <td>962</td>
      <td>1476</td>
      <td>-56.186860</td>
      <td>-34.905610</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>210</th>
      <td>263</td>
      <td>1476</td>
      <td>-56.083195</td>
      <td>-34.883390</td>
      <td>2018-10-02 14:39:23</td>
    </tr>
  </tbody>
</table>
</div>



Podemos ver que el problema esta ocacioando por tener tiempos de varios omnibus, asi que intentamos graficarlos nuevamente pero filtrando por id.


```python
print('Unique ids for line 1476 are:')
line_1476_ids = line_1476.id.unique()
line_1476_ids
```

    Unique ids for line 1476 are:





    array([285, 263, 525, 962, 570, 276, 969, 589, 262, 275])




```python
for id_ in line_1476_ids:
    plot_line(line_1476[line_1476.id == id_])
```


![png](output_30_0.png)



![png](output_30_1.png)



![png](output_30_2.png)



![png](output_30_3.png)



![png](output_30_4.png)



![png](output_30_5.png)



![png](output_30_6.png)



![png](output_30_7.png)



![png](output_30_8.png)



![png](output_30_9.png)


Ahora podemos ver que tenemos recorridos mas coherente. Aún asi vemos que algunos de los recorridos no son muy consistentes entre si.

Esto puede ser por:

* Variantes entre lineas.
* Recorridos de ida deiferentes a los de vuelta.
* Errores en los datos.

Vamos a comparar dos difernetes


```python
line_to_compare_1 = line_1476[line_1476.id == line_1476_ids[4]]
line_to_compare_2 = line_1476[line_1476.id == line_1476_ids[5]]

plt.figure(figsize=(15,5))
plt.subplot(121)
plt.title('Line 1476 id 1')
plt.plot(line_to_compare_1.longitude, line_to_compare_1.latitude, '-o')
plt.subplot(122)
plt.title('Line 1476 id 2')
plt.plot(line_to_compare_2.longitude, line_to_compare_2.latitude, '-o')
plt.show()
plt.figure(figsize=(15,10))
plt.title('Lines 1476 ids 1 & 2 overlap')
plt.plot(line_to_compare_1.longitude, line_to_compare_1.latitude, '-o')
plt.plot(line_to_compare_2.longitude, line_to_compare_2.latitude, '-o', markersize=2)
plt.show()
```


![png](output_33_0.png)



![png](output_33_1.png)


Podemos ver que es la misma linea con la misma trayectoria solo que no completa el recorrido. Esto puede ser por:
* La simulacion se termino antes de obtener los puntos faltantes.
* Es una sublinea


```python
plot_ids_line(line_1476)
```


![png](output_35_0.png)


Podemos comprobar que todas las coordenadas forman parte de la misma linea, aun asi se pueden ver saltos importantes, es decir, periodos de tiempo en los cuales no se obtienen datos de las coorenadas del omnibus.


```python
for line_number in line_numbers[:10]:
    plot_ids_line(lines[line_number])
```


![png](output_37_0.png)



![png](output_37_1.png)



![png](output_37_2.png)



![png](output_37_3.png)



![png](output_37_4.png)



![png](output_37_5.png)



![png](output_37_6.png)



![png](output_37_7.png)



![png](output_37_8.png)



![png](output_37_9.png)


Podemos ver saltos abruptos en casi todas las lineas. Ahora vamos a eliminar coordenadas que esten conectadas con una larga distancia.

Para calcular la distancia entre dos coordenadas vamos a utilizar **Haversine Formula**

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/3547c826fa4abb6bcdf0d4d1de98aaa020ed91d6)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Law-of-haversines.svg/440px-Law-of-haversines.svg.png)


```python
def distance_between(lat1, lon1, lat2, lon2):
    R = 6371e3; #meters
    φ1 = np.deg2rad(lat1)
    φ2 = np.deg2rad(lat2)
    Δφ = np.deg2rad(lat2-lat1)
    Δλ = np.deg2rad(lon2-lon1)

    a = np.sin(Δφ/2)*np.sin(Δφ/2) + np.cos(φ1)*np.cos(φ2)*np.sin(Δλ/2)*np.sin(Δλ/2)
    c = 2*np.arctan2(np.sqrt(a), np.sqrt(1-a))

    d = R * c
    return d
```

Tomemos dos puntos arbitrarios para comprobar la implementacion.


```python
distance_between(-34.890052, -56.187313, -34.887439, -56.132813)
```




    4979.386812255401



Podemos comprobar que el metodo funciona correctamente.

![Haversine](../images/haversine.png)

Ahora tomemos una linea como ejemplo, en este caso 217 y calculemos las distancias entre las coorenadas consecutivas.


```python
line_217 = lines[217]
plot_ids_line(line_217)
```


![png](output_45_0.png)



```python
line_217_ids = line_217.id.unique()
line_217_ids
```




    array([ 76,  68,  15,  13, 105,  91,  10,  45,  34,  29,  20,  90,  82,
            40,  99,  88,   5,  52])




```python
sub_line_217 = pd.DataFrame(line_217[line_217.id == line_217_ids[3]])
plot_line(sub_line_217)
```


![png](output_47_0.png)



```python
sub_line_217.head()
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
      <th>51</th>
      <td>13</td>
      <td>217</td>
      <td>-56.186050</td>
      <td>-34.881200</td>
      <td>2018-10-02 14:39:08</td>
    </tr>
    <tr>
      <th>271</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187170</td>
      <td>-34.880733</td>
      <td>2018-10-02 14:39:23</td>
    </tr>
    <tr>
      <th>394</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187435</td>
      <td>-34.880634</td>
      <td>2018-10-02 14:39:38</td>
    </tr>
    <tr>
      <th>529</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187400</td>
      <td>-34.880615</td>
      <td>2018-10-02 14:39:53</td>
    </tr>
    <tr>
      <th>662</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187584</td>
      <td>-34.880200</td>
      <td>2018-10-02 14:40:08</td>
    </tr>
  </tbody>
</table>
</div>




```python
sub_line_217['next_longitude'] = sub_line_217.longitude.shift(-1)
sub_line_217['next_latitude']  = sub_line_217.latitude.shift(-1)
sub_line_217['next_timestamp'] = sub_line_217.timestamp.shift(-1)
```


```python
sub_line_217.head()
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
      <th>next_longitude</th>
      <th>next_latitude</th>
      <th>next_timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>51</th>
      <td>13</td>
      <td>217</td>
      <td>-56.186050</td>
      <td>-34.881200</td>
      <td>2018-10-02 14:39:08</td>
      <td>-56.187170</td>
      <td>-34.880733</td>
      <td>2018-10-02 14:39:23</td>
    </tr>
    <tr>
      <th>271</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187170</td>
      <td>-34.880733</td>
      <td>2018-10-02 14:39:23</td>
      <td>-56.187435</td>
      <td>-34.880634</td>
      <td>2018-10-02 14:39:38</td>
    </tr>
    <tr>
      <th>394</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187435</td>
      <td>-34.880634</td>
      <td>2018-10-02 14:39:38</td>
      <td>-56.187400</td>
      <td>-34.880615</td>
      <td>2018-10-02 14:39:53</td>
    </tr>
    <tr>
      <th>529</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187400</td>
      <td>-34.880615</td>
      <td>2018-10-02 14:39:53</td>
      <td>-56.187584</td>
      <td>-34.880200</td>
      <td>2018-10-02 14:40:08</td>
    </tr>
    <tr>
      <th>662</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187584</td>
      <td>-34.880200</td>
      <td>2018-10-02 14:40:08</td>
      <td>-56.187380</td>
      <td>-34.879116</td>
      <td>2018-10-02 14:40:23</td>
    </tr>
  </tbody>
</table>
</div>




```python
sub_line_217['distance'] = np.zeros(len(sub_line_217))
sub_line_217['distance'] = sub_line_217.apply(lambda x: distance_between(sub_line_217.latitude,
                                                                          sub_line_217.longitude,
                                                                          sub_line_217.next_latitude,
                                                                          sub_line_217.next_longitude))
```


```python
sub_line_times  = []
sub_line_speeds = []
sub_line_217.next_timestamp.iloc[-1] = sub_line_217.timestamp.iloc[-2]

for row in sub_line_217.itertuples():
    start_time = datetime.strptime(row.timestamp,'%Y-%m-%d %H:%M:%S')
    end_time   = datetime.strptime(row.next_timestamp,'%Y-%m-%d %H:%M:%S')
    time = end_time - start_time
    speed = row.distance/time.total_seconds()
    sub_line_times.append(time)
    sub_line_speeds.append(speed)

sub_line_217['time'] = np.zeros(len(sub_line_217))
sub_line_217['time'] = sub_line_times
sub_line_217['speed'] = np.zeros(len(sub_line_217))
sub_line_217['speed'] = sub_line_speeds
del sub_line_times
del sub_line_speeds
```

    /Users/decemberlabs/anaconda3/lib/python3.5/site-packages/pandas/core/indexing.py:189: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      self._setitem_with_indexer(indexer, value)



```python
sub_line_217.head()
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
      <th>next_longitude</th>
      <th>next_latitude</th>
      <th>next_timestamp</th>
      <th>distance</th>
      <th>time</th>
      <th>speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>51</th>
      <td>13</td>
      <td>217</td>
      <td>-56.186050</td>
      <td>-34.881200</td>
      <td>2018-10-02 14:39:08</td>
      <td>-56.187170</td>
      <td>-34.880733</td>
      <td>2018-10-02 14:39:23</td>
      <td>114.603679</td>
      <td>00:00:15</td>
      <td>7.640245</td>
    </tr>
    <tr>
      <th>271</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187170</td>
      <td>-34.880733</td>
      <td>2018-10-02 14:39:23</td>
      <td>-56.187435</td>
      <td>-34.880634</td>
      <td>2018-10-02 14:39:38</td>
      <td>26.561393</td>
      <td>00:00:15</td>
      <td>1.770760</td>
    </tr>
    <tr>
      <th>394</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187435</td>
      <td>-34.880634</td>
      <td>2018-10-02 14:39:38</td>
      <td>-56.187400</td>
      <td>-34.880615</td>
      <td>2018-10-02 14:39:53</td>
      <td>3.828375</td>
      <td>00:00:15</td>
      <td>0.255225</td>
    </tr>
    <tr>
      <th>529</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187400</td>
      <td>-34.880615</td>
      <td>2018-10-02 14:39:53</td>
      <td>-56.187584</td>
      <td>-34.880200</td>
      <td>2018-10-02 14:40:08</td>
      <td>49.103492</td>
      <td>00:00:15</td>
      <td>3.273566</td>
    </tr>
    <tr>
      <th>662</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187584</td>
      <td>-34.880200</td>
      <td>2018-10-02 14:40:08</td>
      <td>-56.187380</td>
      <td>-34.879116</td>
      <td>2018-10-02 14:40:23</td>
      <td>121.963289</td>
      <td>00:00:15</td>
      <td>8.130886</td>
    </tr>
  </tbody>
</table>
</div>




```python
sub_line_217.drop(['next_latitude','next_longitude','next_timestamp'], axis=1, inplace=True)
sub_line_217.head()
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
      <th>distance</th>
      <th>time</th>
      <th>speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>51</th>
      <td>13</td>
      <td>217</td>
      <td>-56.186050</td>
      <td>-34.881200</td>
      <td>2018-10-02 14:39:08</td>
      <td>114.603679</td>
      <td>00:00:15</td>
      <td>7.640245</td>
    </tr>
    <tr>
      <th>271</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187170</td>
      <td>-34.880733</td>
      <td>2018-10-02 14:39:23</td>
      <td>26.561393</td>
      <td>00:00:15</td>
      <td>1.770760</td>
    </tr>
    <tr>
      <th>394</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187435</td>
      <td>-34.880634</td>
      <td>2018-10-02 14:39:38</td>
      <td>3.828375</td>
      <td>00:00:15</td>
      <td>0.255225</td>
    </tr>
    <tr>
      <th>529</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187400</td>
      <td>-34.880615</td>
      <td>2018-10-02 14:39:53</td>
      <td>49.103492</td>
      <td>00:00:15</td>
      <td>3.273566</td>
    </tr>
    <tr>
      <th>662</th>
      <td>13</td>
      <td>217</td>
      <td>-56.187584</td>
      <td>-34.880200</td>
      <td>2018-10-02 14:40:08</td>
      <td>121.963289</td>
      <td>00:00:15</td>
      <td>8.130886</td>
    </tr>
  </tbody>
</table>
</div>




```python
sub_line_217.speed.mean()
```




    4.028899993403776




```python
plot_line(sub_line_217)
```


![png](output_56_0.png)



```python
plot_line_speed(sub_line_217)
```


![png](output_57_0.png)


Podemos ver que algunas partes del camino se salen del recorrido, esto puede deberse a un desvio. Para conseguir mas información vamos a intentar mapear la id de las lineas a la linea del omnibus real. Para esto vamos a intruducir un nuevo archivo, `uv_uptu_lsv.csv` obtenido de los datos abiertos de la IMM (intendencia municipal de Montevideo)


```python
uptu_data = pd.read_csv('../data/v_uptu_lsv.csv')
uptu_data.head()
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
      <th>GID</th>
      <th>COD_LINEA</th>
      <th>DESC_LINEA</th>
      <th>ORDINAL_SU</th>
      <th>COD_SUBLIN</th>
      <th>DESC_SUBLI</th>
      <th>COD_VARIAN</th>
      <th>DESC_VARIA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16009404</td>
      <td>0</td>
      <td>LINEA CERO</td>
      <td>1</td>
      <td>0</td>
      <td>SUBLINEA CERO</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16009406</td>
      <td>1</td>
      <td>402</td>
      <td>1</td>
      <td>1</td>
      <td>CIUDAD VIEJA - MALVIN</td>
      <td>8</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16009407</td>
      <td>2</td>
      <td>404</td>
      <td>1</td>
      <td>2</td>
      <td>CJO.J.AM├ëRICA - PCIO.DE LA LUZ</td>
      <td>14</td>
      <td>B</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16009408</td>
      <td>2</td>
      <td>404</td>
      <td>1</td>
      <td>2</td>
      <td>CJO.J.AM├ëRICA - PCIO.DE LA LUZ</td>
      <td>20</td>
      <td>A</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16009409</td>
      <td>3</td>
      <td>405</td>
      <td>1</td>
      <td>3</td>
      <td>PE├æAROL - PARQUE ROD├ô</td>
      <td>24</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>



Este archivo contiene:

* **Gid**         Código identificador de uso interno
* **Cod_linea**   Código de la línea de transporte
* **Desc_linea**  Descripción de la línea de transporte (ej: 145, D10, etc)
* **Ordinal_su**  Número correlativo de la sublinea en la línea
* **Cod_sublin**  Código de la sublínea de recorrido
* **Desc_subli**  Descripción de la sublínea de recorrido (ej: ADUANA-PORTONES)
* **Cod_varian**  Código de la variante de recorrido (para vincular con v_uptu_parada)
* **Desc_varia**  Descripción de la variante (Se asigna sentido A, a todas las líneas que circulan con destino hacia la zona sur del departamento (Ciudad Vieja, Centro, Cordón, P. Rodó, P. Carretas, Pocitos, Buceo, Malvín), además de todas las líneas que atraviesan esta zona en sentido Oeste-Este. Se asigna sentido B, a todas las líneas en sentido contrario al antes mencionado.

Vamos a eliminar la primera fila ya que no aprota informacion, el codigo de uso interno, ordinal, y codigo de linea.


```python
uptu_data.drop(['GID','COD_LINEA','ORDINAL_SU'], axis=1, inplace=True)
uptu_data.drop(uptu_data.index[0], inplace=True)
uptu_data.head()
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
      <th>DESC_LINEA</th>
      <th>COD_SUBLIN</th>
      <th>DESC_SUBLI</th>
      <th>COD_VARIAN</th>
      <th>DESC_VARIA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>402</td>
      <td>1</td>
      <td>CIUDAD VIEJA - MALVIN</td>
      <td>8</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2</th>
      <td>404</td>
      <td>2</td>
      <td>CJO.J.AM├ëRICA - PCIO.DE LA LUZ</td>
      <td>14</td>
      <td>B</td>
    </tr>
    <tr>
      <th>3</th>
      <td>404</td>
      <td>2</td>
      <td>CJO.J.AM├ëRICA - PCIO.DE LA LUZ</td>
      <td>20</td>
      <td>A</td>
    </tr>
    <tr>
      <th>4</th>
      <td>405</td>
      <td>3</td>
      <td>PE├æAROL - PARQUE ROD├ô</td>
      <td>24</td>
      <td>B</td>
    </tr>
    <tr>
      <th>5</th>
      <td>405</td>
      <td>246</td>
      <td>GRUTA DE LOURDES - PARQUE RODO</td>
      <td>28</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>



Ahora vamos a ver cual es el nombre real de la linea con la que trabajamos _217_


```python
uptu_data[uptu_data.COD_VARIAN==217]
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
      <th>DESC_LINEA</th>
      <th>COD_SUBLIN</th>
      <th>DESC_SUBLI</th>
      <th>COD_VARIAN</th>
      <th>DESC_VARIA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>546</td>
      <td>22</td>
      <td>BELVEDERE - PORTONES</td>
      <td>217</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>



Vemos que es el 546, buscamos su recorrido y efectivamente comprobamos que los datos son correctos

![546](../images/line_546.png)

En la proxima sección vamos a realizar estos ultimmos pasos para todos los datos y obtendremos distancias y velocidades.
