# K-Means Clustering

### K-Means Consideraciones

### Proceso en RapidMiner

__Seed = 2018__


1- Agregamos el dataset en un proceso nuevo con el modulo `Retrive`.

2- Eliminamos los atributos inecesarios con un modulo de `Select Attributes`, en este caso vamos a eliminar la id y la clase (`Class`).

3- Como vimos en [Missing Values](./), este dataset contiene valores faltantes en el atributo **Bare Nuclei**. Vamos a removerlos con el modulo `Filter Examples`.

4- En los proximos pasos vamos a utlizar el modulo de `Performace (Classification)` el cual requiere que las _labels_ sean de tipo _polynomial_. Actualmente nuestras variables de salida son 2 y 4, para cumplir con este requsito vamos a utilizar el modulo `Numerical to Polynominal` sobre la variable **Class**.

5- Los valores del atributo **Bare Nuclei** estan siendo considerados como _polynomial_ vamos a utilizar el modulo de `Nominal to Numerical` para convertirlo en numeros.

El paso 4 y 5 los englobamos en un `Subprocess`.

6- Indicamos que el atributo **Class** va a ser nuestra _label_ a predecir con el modulo `Set Role`.

7- Para evaluar que tan buneo es nuestro modelo vamos a utilizar validacion cruza tambien conocido como _cross validation_, vamos a utilizar el estandar de 10 folds.

* Dentro del cross validation:
  
  7.1- En el lado izquierdo _(training)_ agregamos el modulo `Naive Bayes`.

  7.2- En el lado derecho _(testing)_ agregamos el modulo de `Apply model` conectado a `Performance (Classification)`.

