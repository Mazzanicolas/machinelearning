
# K-Nearest Neighbors

En este caso vamos a estar usando K-NN para clasificacion por lo cual la salida va a ser calculada como la moda en los K datos mas cercanos.

### importamos las librerias necesarias


```python
import pandas as pd
import pandas_profiling as pdp
import numpy as np
```


```python
data = pd.read_csv('breast-cancer.csv')
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
      <th>id</th>
      <th>Clump_Thickness</th>
      <th>Uniformity_of_Cell_Size</th>
      <th>Uniformity_of_Cell_Shape</th>
      <th>Marginal_Adhesion</th>
      <th>Single_Epithelial_Cell_Size</th>
      <th>Bare_Nuclei</th>
      <th>Bland_Chromatin</th>
      <th>Normal_Nucleoli</th>
      <th>Mitoses</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000025</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002945</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>7</td>
      <td>10</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1015425</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1016277</td>
      <td>6</td>
      <td>8</td>
      <td>8</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>7</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1017023</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



### Missing Values

Como ya vimos en la seccion de [missing values](./) Bare Nuclei tiene missing values por lo cual los eliminamos.


```python
data = data.drop(['id'], axis=1)

data = data[data.Bare_Nuclei!='?']

data.Bare_Nuclei = data.Bare_Nuclei.apply(lambda x: int(x))
```


```python
data.describe()
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
      <th>Bland_Chromatin</th>
      <th>Normal_Nucleoli</th>
      <th>Mitoses</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4.442167</td>
      <td>3.150805</td>
      <td>3.215227</td>
      <td>2.830161</td>
      <td>3.234261</td>
      <td>3.445095</td>
      <td>2.869693</td>
      <td>1.603221</td>
      <td>2.699854</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.820761</td>
      <td>3.065145</td>
      <td>2.988581</td>
      <td>2.864562</td>
      <td>2.223085</td>
      <td>2.449697</td>
      <td>3.052666</td>
      <td>1.732674</td>
      <td>0.954592</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>4.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>6.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
      <td>4.000000</td>
      <td>4.000000</td>
      <td>5.000000</td>
      <td>4.000000</td>
      <td>1.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>4.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
class KNN:
    def __init__(self):
        pass

    def train(self, X, y):
        self.X_train = X
        self.y_train = y
       
    def predict(self, X, k=1):
        """
        Calculamos la distancia L2 entre los datos de test y validacion, devolviendo la moda en k
        """
        distances = []
        predictions = []
        for test_sample in X:
            for index, train_sample in enumerate(self.X_train):
                distances.append(np.linalg.norm(test_sample-train_sample))
            k_closest = np.take(self.y_train, np.argsort(distances)[:k])
            counts = np.bincount(k_closest)
            y_pred = np.argmax(counts)
            predictions.append(y_pred)
            distances = []
        return predictions
```


```python
from sklearn.model_selection import train_test_split

# Creamos una instancia de nuestro clasificador KNN
knn = KNN()
# Separamos la label de los datos
y = data.Class
X = data.drop(['Class'], axis=1)
# Hacemos un split 
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.30, random_state=0)
# Entrenamos nuestro modelo
knn.train(X_train.values, y_train.values)
# Hacemos las predicciones
predictions = knn.predict(X_test.values, 5)
# Calculamos el error
differences = abs(predictions - y_test.values)
unique, predictions = np.unique(differences, return_counts=True)
correct_predictions = predictions[0]
wrong_predictions = predictions[1]
# Calculamos la accuracy
accuracy = correct_predictions/(correct_predictions+wrong_predictions)

print('Accuracy: {0}'.format(accuracy))
```

    Accuracy: 0.9609756097560975


Podemos ver que se llega a una accuracy relativamente alta.
