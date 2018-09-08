# Height Estimation from Skeletal Remains 
 
![Height Estimation from Skeletal
Remains](./2_src/img/banner.jpg) 

## Overview

## Dataset

### Cargando el dataset

El dataset se encuentra en un archivo `.accdb` (Microsoft Access) asi que lo primero que vamos a hacer es exportaro como `.csv`

![Acces DB](./2_src/img/accesdb.png)

Analizandolo rapidamente en un editor de texto podemos ver que contiene carateres que no van a poder ser decodeados con UTF-8 (`�`). 

![UTF-8 Error](./2_src/img/utf8Error.png)

Ahora ya podemos cargarlo con `pandas`

[Vista detallada del dataset ➡](./2_src/pf_overview.html)



```python
import missingno as msno
import pandas as pd
import pandas_profiling as pf
```


```python
data = pd.read_csv('../dataindsamling.csv')
```


```python
msno.matrix(data)
```


![png](./2_src/img/output_2_1.png)



```python
msno.bar(data)
```


![png](./2_src/img/output_3_1.png)

