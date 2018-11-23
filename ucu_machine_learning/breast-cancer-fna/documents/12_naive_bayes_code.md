
# Naive Bayes

Vamos a utilizar el clasificador Naive bayes para predecir las categorias Maligno / Benigno.

Naive bayes asume que las clases a predecir son categoricas, requisito que se cumple con este dataset.

### Importando librerias



```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
```


```python
# Cargamos los datos
data = pd.read_csv('../dataset/breast-cancer.csv')
# Eliminamos los atributos que no vamos a utilzar
data = data.drop(['id'], axis=1)
data = data[data.Bare_Nuclei!='?']
# Convertimos a valores numericos Bare Nuclei
data.Bare_Nuclei = data.Bare_Nuclei.apply(lambda x: int(x))
```


```python
# Separamos los datos en validacion y test
y = data.Class
X = data.drop(['Class'], axis=1)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.30, random_state=0)

# Creamos un clasificador
classifier = MultinomialNB()
classifier.fit(X_train, y_train)

# Realizamos las predicciones
predictions = classifier.predict(X_test)

# Calculamos la accuracy
differences = abs(predictions - y_test.values)
unique, predictions = np.unique(differences, return_counts=True)
correct_predictions = predictions[0]
wrong_predictions = predictions[1]
# Calculamos la accuracy
accuracy = correct_predictions/(correct_predictions+wrong_predictions)

print('Accuracy: {0}'.format(accuracy))
```

    Accuracy: 0.8878048780487805



```python
# Para comprobar que calculamos bien la accuracy utilizamos el metodo incorporado en sklearn
print('Accuracy: {0}'.format(classifier.score(X_test, y_test)))
```

    Accuracy: 0.8878048780487805


Para ver una analisis mas detallado de ver [Naive Bayes RapidMiner](./)
