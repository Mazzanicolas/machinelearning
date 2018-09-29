


<!-- Correlacion en mitosis? -->
## Analizando el Dataset (RapidMiner)

Primero cargamos los rapidamente los datos en RapidMiner.

![Dataset](./1_src/img/rapidminer_load.png)

Ahora miramos las estadisticas para poder ver las distribuciones e identificar posibles outliers.

![Dataset](./1_src/img/stats_1.png)
![Dataset](./1_src/img/stats_2.png)

Podemos ver que casi todos los datos menos Clump Thickness tienen una distribucion similar (Similar a una beta).
Tambien podemos ver que no se indican missing values pero que `Bare Nuclei` es de tipo `Polynomial` cuando el dataset indicaba que todos los atributos eran numericos, vamos a tener que analizar esto como un posible caso de missing values.

### Missing Values (RapidMiner)

Eliminamos la columna id y nos ponemos a analizar los missing values.

![Dataset](./1_src/img/rapidminer_missing_1.png)

![Dataset](./1_src/img/rapidminer_missing_2.png)

Podemos ver que en `Bare Nuclei` tenemos valores de `?`, RapidMiner no los clasifica como missing values porque interpreta `?` como un valor posible del atributo. Procedemos a eliminar las filas con esos missing values ya que no tenemos como estimarlos y son solo 16 casos en 699.

![Dataset](./1_src/img/rapidminer_missing_3.png)

### Correlacion (RapidMiner)

Como `Bare Nuclei` estaba como valor `Polynomial` a pesar de haber removido los missing values tenemos que convertlirlos nuevamente en valores numericos. Una vez convertidos podemos hacer una matriz de correlaciones.

![Dataset](./1_src/img/rapidminer_corr_1.png)

![Dataset](./1_src/img/rapidminer_corr_2.png)

Podemos ver una alta correlacion entre `Uniformity of Cell Shape` y `Uniformity of Cell Size`, como vimos en el estudio de atributos esto tiene sentido dado que al ser mas grandes las celulastienen una forma mas irregular.


