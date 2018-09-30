# Missing Values

Eliminamos la columna id y nos ponemos a analizar los missing values.

![Dataset](./img/rapidminer_missing_1.png)

![Dataset](./img/rapidminer_missing_2.png)

Como vimos en el análisis de datos no se indican missing values pero que `Bare Nuclei` es de tipo `Polynomial` cuando el dataset indicaba que todos los atributos eran numericos, vamos a tener que analizar esto como un posible caso de missing values.

Podemos ver que en `Bare Nuclei` tenemos valores de `?`, RapidMiner no los clasifica como missing values porque interpreta `?` como un valor posible del atributo. Procedemos a eliminar las filas con esos missing values ya que no tenemos como estimarlos y son solo 16 casos en 699.

![Dataset](./img/rapidminer_missing_3.png)