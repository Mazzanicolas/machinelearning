# K-Nearest Neighbors 

### K-NN Consideraciones

KNN es un algoritmo que en la etapa de entrenamiento lo unico que hace es almacenar todos los datos y al momento de predecir se calcula la distancia del dato de entrada contra todos los datos almacenados previamente, luego se regresan las K distancias mas cercanas.

✔ Dataset muy grandes ocupan mucho espacio.
* No vamos a tener problema ya que contamos con ~700 datos.

✔ Como se calculan distancias siempre es bueno tener los datos normalizados en intervalos similares para mejores resultados.
*  Los datos ya se encuentran en un intervalo de [0 - 10] por lo cual no va a ser necesario reescalarlos.

✔ Es importante utilizar una medida de distancia adecuada.
* En este caso vamos a utilizar la distancia L2.

![L2](https://wikimedia.org/api/rest_v1/media/math/render/svg/795b967db2917cdde7c2da2d1ee327eb673276c0)

* Tambien vamos a experimentar con la distancia Manhattan.

![Manhattan](https://wikimedia.org/api/rest_v1/media/math/render/svg/02436c34fc9562eb170e2e2cfddbb3303075b28e)

✔ KNN puede ser utilizado para problemas de clasificación o regresión.
* En este caso lo vamos a utilizar para clasificar un tumor como Maligno o Benigno.

### Proceso en RapidMiner

__Seed = 2018__

1- Agregamos el dataset en un proceso nuevo con el modulo `Retrive`.

2- Eliminamos los atributos inecesarios con un modulo de `Select Attributes`, en este caso vamos a eliminar la id.

3- Como vimos en [Missing Values](./), este dataset contiene valores faltantes en el atributo **Bare Nuclei**. Vamos a removerlos con el modulo `Filter Examples`.

4- En los proximos pasos vamos a utlizar el modulo de `Performace (Classification)` el cual requiere que las _labels_ sean de tipo _polynomial_. Actualmente nuestras variables de salida son 2 y 4, para cumplir con este requsito vamos a utilizar el modulo `Numerical to Polynominal` sobre la variable **Class**.

5- Los valores del atributo **Bare Nuclei** estan siendo considerados como _polynomial_ vamos a utilizar el modulo de `Nominal to Numerical` para convertirlo en numeros.

El paso 4 y 5 los englobamos en un `Subprocess`.

6- Indicamos que el atributo **Class** va a ser nuestra _label_ a predecir con el modulo `Set Role`.

7- Para evaluar que tan buneo es nuestro modelo vamos a utilizar validacion cruza tambien conocido como _cross validation_, vamos a utilizar el estandar de 10 folds.

* Dentro del cross validation:
  
  7.1- En el lado izquierdo _(training)_ agregamos el modulo `k-NN`.
  
  7.2- En el lado derecho _(testing)_ agregamos el modulo de `Apply model` conectado a `Performance (Classification)`.

Al momento de utilziar K-NN un hiperparametro importante es **K**, la cantidad de vecinos. A continuacion vamos a probar con varios K y observar los resultados.

### Process

![](./img/11_knn_process_1.png)

### Cross Validation

![](./img/11_knn_process_2.png)


## Experimentos

**Notas:**

* Se omite K=1 porque sobre ajusta el modelo a los datos de entrenamiento.
* Como se tiene un numero par de clases lo ideal seria tomar un K impar.

| K  | Measure            | Accuracy         | 2 Recall | 4 Recall |
|----| ------------------ | ---------------- | -------- | -------- |
| 2  | Euclidean Distance | 94.29% +/- 3.04% |  96.85%  |  89.54%  |
| 3  | Euclidean Distance | 96.20% +/- 2.08% |  97.30%  |  94.14%  |
| 4  | Euclidean Distance | 95.76% +/- 2.11% |  96.85%  |  93.72%  |
| 5  | Euclidean Distance | 96.20% +/- 2.09% |  96.62%  |  95.40%  |
| 6  | Euclidean Distance | 95.90% +/- 2.05% |  96.62%  |  94.56%  |
| 7  | Euclidean Distance | 95.76% +/- 2.31% |  96.40%  |  94.56%  |
| 8  | Euclidean Distance | 95.90% +/- 2.25% |  96.40%  |  94.98%  |
| 9  | Euclidean Distance | 95.90% +/- 2.25% |  96.40%  |  94.98%  |
| 10 | Euclidean Distance | 95.90% +/- 2.25% |  96.40%  |  94.98%  |
| 2  | Manhattan Distance | 95.75% +/- 2.82% |  97.75%  |  92.05%  |
| 3  | Manhattan Distance | 96.49% +/- 2.47% |  96.85%  |  95.82%  |
| 4  | Manhattan Distance | 97.07% +/- 1.96% |  97.52%  |  96.23%  |
| **5**  | **Manhattan Distance** | **96.78% +/- 1.95%** |  **97.07%**  |  **96.23%**  |
| 6  | Manhattan Distance | 96.92% +/- 1.79% |  97.07%  |  96.65%  |
| 7  | Manhattan Distance | 96.78% +/- 2.16% |  97.07%  |  96.23%  |
| 8  | Manhattan Distance | 96.49% +/- 2.10% |  97.07%  |  95.40%  |
| 9  | Manhattan Distance | 96.34% +/- 2.29% |  97.07%  |  94.98%  |
| 10 | Manhattan Distance | 96.34% +/- 1.99% |  97.07%  |  94.98%  |
| 11 | Manhattan Distance | 96.34% +/- 1.99% |  97.07%  |  94.98%  |

Podemos ver que el modelo con distancia Euclideana y K>8, cuando utilizamos la distancia Manhattan se estanca con K>9. Nuestro mejor modelo es con distancia Manhattan y K=4, tiene un buena accuracy/recall y 4 es una cantidad de vecinos razonables, por otra parte es preferible (en este caso) tener un k impar, por eso nos tomamos K=5.
