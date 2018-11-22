
# Logistic Regression

_Este codigo es una continuación de [correlation code](./6_correlation_code.md)_.

Para hacer regresión logistica vamos a importar algunas librerias de sklearn.


```python
from sklearn import preprocessing
from sklearn.feature_selection import RFE
from sklearn.linear_model import LogisticRegression
from sklearn.cross_validation import train_test_split
sns.set(style="white")
sns.set(style="whitegrid", color_codes=True)
```

Para poder realizar la regresión logistica tenemos que tener en cuenta algunas considreaciones:

1 - La regresión logistica requiere que la variable dependiente sea binaria


```python
dataset[' Class'].value_counts()
```




    2    444
    4    239
    Name:  Class, dtype: int64



2 - Eliminar rudio y outliers

3 - Eliminar atributos correlacionados


```python
dataset.head()
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
      <th>Clump Thickness</th>
      <th>Uniformity of Cell Size</th>
      <th>Marginal Adhesion</th>
      <th>Single Epithelial Cell Size</th>
      <th>Bare Nuclei</th>
      <th>Bland Chromatin</th>
      <th>Normal Nucleoli</th>
      <th>Mitoses</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
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
      <td>5</td>
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
      <td>3</td>
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
      <td>6</td>
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
      <td>4</td>
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



Ahora vamos a separar en train y test sets


```python
labels = dataset.iloc[:,-1:] 
data_  = dataset.iloc[:,:-1]
```


```python
data_.head(2)
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
      <th>Clump Thickness</th>
      <th>Uniformity of Cell Size</th>
      <th>Marginal Adhesion</th>
      <th>Single Epithelial Cell Size</th>
      <th>Bare Nuclei</th>
      <th>Bland Chromatin</th>
      <th>Normal Nucleoli</th>
      <th>Mitoses</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>7</td>
      <td>10</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
labels.head(2)
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
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



Vamos a ver cuantas ocurrencias tenemos en cada clase


```python
sns.countplot(x=labels[' Class'], data=labels, palette='hls')
plt.show()
```


![png](./img/output_52_0.png)



```python
dataset.groupby(' Class').mean()
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
      <th>Clump Thickness</th>
      <th>Uniformity of Cell Size</th>
      <th>Marginal Adhesion</th>
      <th>Single Epithelial Cell Size</th>
      <th>Bland Chromatin</th>
      <th>Normal Nucleoli</th>
      <th>Mitoses</th>
    </tr>
    <tr>
      <th>Class</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>2.963964</td>
      <td>1.306306</td>
      <td>1.346847</td>
      <td>2.108108</td>
      <td>2.083333</td>
      <td>1.261261</td>
      <td>1.065315</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.188285</td>
      <td>6.577406</td>
      <td>5.585774</td>
      <td>5.326360</td>
      <td>5.974895</td>
      <td>5.857741</td>
      <td>2.602510</td>
    </tr>
  </tbody>
</table>
</div>



Podemos ver las diferencias entre los valores por lo cual no vale la pena sacar ninguno

Separamos nuestro dataset en `entrenamiento` y `test`.


```python
X_train, X_test, y_train, y_test = train_test_split(data_, labels,
                                                    test_size=0.3,
                                                    random_state=0)
```

Creamos un modelo de regresión logistica y lo entrenamos.


```python
logisticRegr = LogisticRegression()
logisticRegr.fit(X_train, y_train.values.reshape(478,))
```




    LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)



Realizamos predicciones con nuestro modelo y comparamos los resutados obtenidos contra los esperados.


```python
prediction = logisticRegr.predict(X_test)
columns = ['Predicted Value', 'Actual Value']
results = pd.DataFrame()
results['Predicted Value'] = prediction
results['Actual Value']    = y_test.values
```


```python
results.head()
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
      <th>Predicted Value</th>
      <th>Actual Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



Ahora calculamos la `accuracy` de nuestro modelo.




```python
wrong_predictions = results[results['Predicted Value'] != results['Actual Value']]
accuracy = (len(results)-len(wrong_predictions))/len(results)
accuracy
```




    0.9463414634146341

