
# PCA

Importando librerias


```python
import matplotlib
import seaborn as sns
import pandas as pd
from sklearn.decomposition import PCA
```


```python
# Cargamos los datos
data = pd.read_csv('../dataset/breast-cancer.csv')
# Eliminamos los atributos que no vamos a utilizar
data = data.drop(['id'], axis=1)
data = data[data.Bare_Nuclei!='?']
# Convertimos los valores de Bare Nuclei en enteros
data.Bare_Nuclei = data.Bare_Nuclei.apply(lambda x: int(x))
```


```python
# Eliminamos la label
X = data.drop(['Class'], axis=1)
# Entremamos un PCA
pca = PCA(n_components=5)
pca.fit(X)
# Graficamos los componentes
results = pd.DataFrame(pca.components_, columns=X.columns)
results
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
      <th>Clump_Thickness</th>
      <th>Uniformity_of_Cell_Size</th>
      <th>Uniformity_of_Cell_Shape</th>
      <th>Marginal_Adhesion</th>
      <th>Single_Epithelial_Cell_Size</th>
      <th>Bare_Nuclei</th>
      <th>Bland_Chromatin</th>
      <th>Normal_Nucleoli</th>
      <th>Mitoses</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.296736</td>
      <td>0.403971</td>
      <td>0.392759</td>
      <td>0.331202</td>
      <td>0.249740</td>
      <td>0.442613</td>
      <td>0.292078</td>
      <td>0.354536</td>
      <td>0.124576</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.073507</td>
      <td>0.229929</td>
      <td>0.164701</td>
      <td>-0.098198</td>
      <td>0.200215</td>
      <td>-0.780570</td>
      <td>0.008480</td>
      <td>0.469195</td>
      <td>0.188069</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.852009</td>
      <td>-0.026274</td>
      <td>-0.074455</td>
      <td>0.473858</td>
      <td>0.031656</td>
      <td>0.093418</td>
      <td>0.122412</td>
      <td>0.133724</td>
      <td>0.026631</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.047064</td>
      <td>0.294619</td>
      <td>0.206515</td>
      <td>0.394199</td>
      <td>0.188823</td>
      <td>-0.294384</td>
      <td>-0.041223</td>
      <td>-0.753949</td>
      <td>0.143139</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.399765</td>
      <td>0.337969</td>
      <td>0.370151</td>
      <td>-0.642499</td>
      <td>0.195054</td>
      <td>0.168674</td>
      <td>0.092590</td>
      <td>-0.196890</td>
      <td>-0.249623</td>
    </tr>
  </tbody>
</table>
</div>


