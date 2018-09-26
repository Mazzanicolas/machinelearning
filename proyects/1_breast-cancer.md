# Breast Cancer Wisconsin

![Breast Cancer](./1_src/img/banner.jpg)

En este proyecto se va a realizar un estudio sobre el dataset [Breast Cancer Wisconsin](https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+%28Original%29), obtenido de [UCI](https://archive.ics.uci.edu/ml/index.php).

## Introducción 
Las células cancerígenas se crean cuando los genes responsables por la división celular están dañados. La carcinogénesis (conjunto de fenómenos que determinan la aparición y desarrollo de un cáncer) es causada por mutación y epimutación (cambio en la estructura química del ADN que no altera la secuencia de codificación del ADN) del material genético en las células normales, las cuales desequilibran el balance del material genético en las células normales entre la proliferación y muerte de las células. Esto resulta en una división celular fuera de control y la evolución de esas células por selección natural. La descontrolada y en la mayoría de los casos, rápida proliferación de las células puede llevar a tumores benignos o malignos (cáncer). Los tumores benignos no invaden otras partes del cuerpo o otros tejidos, mientras que los tumores malignos pueden invadir otros órganos y llegar a lugares lejanos en el cuerpo (metastasis), convirtiéndose asi, en una amenaza le.
<!--
Cancer cells are created when the genes responsible for regulating [cell division](https://en.wikipedia.org/wiki/Cell_division "Cell division") are damaged. Carcinogenesis is caused by mutation and epimutation of the genetic material of normal cells, which upsets the normal balance between proliferation and cell death. This results in uncontrolled cell division and the [evolution of those cells](https://en.wikipedia.org/wiki/Somatic_evolution_in_cancer "Somatic evolution in cancer") by [natural selection](https://en.wikipedia.org/wiki/Natural_selection "Natural selection") in the body. The uncontrolled and often rapid proliferation of cells can lead to benign or malignant tumours (cancer). [Benign tumors](https://en.wikipedia.org/wiki/Benign_tumor "Benign tumor") do not spread to other parts of the body or invade other tissues. [Malignant tumors](https://en.wikipedia.org/wiki/Malignant_tumor) can invade other organs, spread to distant locations ([metastasis](https://en.wikipedia.org/wiki/Metastasis "Metastasis")) and become life-threatening.
-->
El dataset ser estudiado esta compuesto por los resultados obtenidos mediante biopsias de ​FNA​ (fine needle aspiration), contiene 699 instancias con 10 atributos y una clase de salida (benigno o malicioso).
En una biopsia FNA, el doctor usa una aguja muy fina y hueca con una jeringa para poder aspirar una pequeña cantidad de tejido o fluido de un area sospechosa. La muestra recolectada es revisada para ver si hay celulas cancerígenas.
![FNA](./1_src/img/fna.gif)
<!--
In an FNA biopsy, the doctor uses a very thin, hollow needle attached to a syringe to withdraw (aspirate) a small amount of tissue or fluid from a suspicious area. The biopsy sample is then checked to see if there are cancer cells in it.
-->
El dataset fue creado por el Dr.  WIlliam H. Wolberg (medico) en los hospitales de la universidad de Wisconsin en Madison, Wisconsin, USA.
Las muestras llegaban periódicamente mientras el Dr. Wolberg reportaba sus casos clínicos. Por esto, los datos están agrupados por orden cronológico, estos fueron recolectados entre Enero de 1989 y Noviembre de 1991.
<!--
This dataset was created by Dr. WIlliam H. Wolberg (physician)  at the University of Wisconsin Hospitals (Madison, Wisconsin, USA).
Samples arrive periodically as Dr. Wolberg reports his clinical cases. The database therefore reflects this chronological grouping of the data. The data groups were collected between January 1989 and November 1991. 
-->

| Grupo  | Instancias | Mes       | Año  |
|--------|------------|-----------|------|
| 1      | 367        | Enero     | 1989 |
| 2      | 70         | Octubre   | 1989 |
| 3      | 31         | Febrero   | 1990 |
| 4      | 17         | Abril     | 1990 |
| 5      | 48         | Agosto    | 1990 |
| 6      | 49         | Enero     | 1991 | 
| 7      | 31         | Junio     | 1991 | 
| 8      | 86         | Noviembre | 1991 | 

Los datos ya fueron limpiados en dos ocasiones.
* *Jan 10, 1991*
* *Nov 22,1991*

Aunque se encuentran missing values en Bare Nuclei como veremos mas adelante.

Un total de 699 instancias.

<!--
* Group 1: 367 instances (January 1989)  
* Group 2: 70 instances (October 1989)  
* Group 3: 31 instances (February 1990)  
* Group 4: 17 instances (April 1990)  
* Group 5: 48 instances (August 1990)  
* Group 6: 49 instances (Updated January 1991)  
* Group 7: 31 instances (June 1991)  
* Group 8: 86 instances (November 1991)

A total of 699 instances.
-->

## Problema

El problema consiste en predecir si una muestra de células recolectada mediante ​FNA​ es benigna o no en base a una serie de atributos.

## Estudio de atributos

| Atributo                    | Valor Minimo | Valor Maximo  |
|-----------------------------|--------------|---------------|
| Sample code number          | 61634        | 13454352      |
| Clump Thickness             | 1            | 10            |
| Uniformity of Cell Size     | 1            | 10            |
| Uniformity of Cell Shape    | 1            | 10            |
| Marginal Adhesion           | 1            | 10            |
| Single Epithelial Cell Size | 1            | 10            |
| Bare Nuclei                 | 1            | 10            |
| Bland Chromatin             | 1            | 10            |
| Normal Nucleoli             | 1            | 10            |
| Mitoses                     | 1            | 10            |
| Class                       | 2 (Benigno)  | 4 (Maligno)   |  

La variable objetivo de este dataset es “Class” la cual determina si un tumor es benigno o maligno (con el valor 2 o 4 respectivamente).

A simple vista podemos descartar el atributo id ya que no aporta valor y causa ruido. 

![Normal cell vs Cancer cell](./1_src/img/cell_structure.jpg)

A continuación una breve descripción de las definiciones  para los atributos del dataset y porque pueden ser relevantes.

**Clump Thickness:** `​Grosor de la masa a analizar`
*Normalmente las células benignas suelen estar agrupadas en una única capa mientras que las malignas se agrupan en más de una por lo cual el grosor parece ser un factor importante al momento de clasificar.*

**Uniformity of Cell Size:** `​Uniformidad del tamaño en las células extraídas` 
*Las células cancerígenas suelen variar en tamaño.*

**Uniformity of Cell Shape:** `​Uniformidad de la forma en las células extraídas`  
*Las células cancerígenas suelen variar en forma.*

Estos últimos dos atributos también parecen ser importantes al momento de realizar la clasificación y pueden estar correlacionados.

**Marginal Adhesion:** `​Adhesión de las células` 
*Las células normales tienen a estar juntas mientras que las cancerígenas tienden a separarse.*

**Single Epithelial Cell Size:** `​Tamaño de las células` 
*Las células cancerígenas tienden a ser más grandes.*

**Bare Nuclei:** ​`Núcleo que no está cubierto por el citoplasma` *Comunes en tumores benignos.*

**Bland Chromatin:** ​`Uniformidad de la textura`
*En las células cancerígenas suele ser más gruesa.*

**Normal Nucleoli:** ​`Son pequeñas estructuras en los núcleos.` 
*En las células normales es apenas visible, o ni siquiera es visible mientras que en las células cancerígenas es más prominente.*

**Mitoses:** ​`División de las células.` 
*En las células cancerígenas suelen ser más asimétricas.*

**Class:** ​`Tipo de tumor`
* *2 para benigno*
* *4 para maligno*

## Analizando el Dataset (código)

[Vista detallada del dataset ➡](./1_src/pf_overview.html)

### Importando librerias

```python
import matplotlib.pyplot as plt
import pandas  as pd
import numpy   as np
import seaborn as sns
```

### Cargando el dataset

```python
dataset = pd.read_csv('breast_cancer.csv')
```

### Eliminando la columna Id number


```python
dataset = dataset.drop('id number', 1)
```
### Missing Values

```python
dataset.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 699 entries, 0 to 698
    Data columns (total 10 columns):
    Clump Thickness                699 non-null int64
    Uniformity of Cell Size        699 non-null int64
    Uniformity of Cell Shape       699 non-null int64
    Marginal Adhesion              699 non-null int64
    Single Epithelial Cell Size    699 non-null int64
    Bare Nuclei                    699 non-null object
    Bland Chromatin                699 non-null int64
    Normal Nucleoli                699 non-null int64
    Mitoses                        699 non-null int64
    Class                          699 non-null int64
    dtypes: int64(9), object(1)
    memory usage: 54.7+ KB
    


```python
missing_values = dataset[dataset['Bare Nuclei']=='?']
```

Podemos verque la variable `Bare Nuclei` tiene por lo menos una ocurrencia con un tipo de dato extraño.

```python
missing_values
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
      <th>Uniformity of Cell Shape</th>
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
      <th>23</th>
      <td>8</td>
      <td>4</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>7</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>40</th>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>6</td>
      <td>?</td>
      <td>7</td>
      <td>8</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>139</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>?</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>145</th>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>158</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>?</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>164</th>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>235</th>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>249</th>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>275</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>292</th>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>6</td>
      <td>10</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>294</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>297</th>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>315</th>
      <td>4</td>
      <td>6</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>?</td>
      <td>4</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>321</th>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>?</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>411</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>?</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>617</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>?</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>

Confirmamos que tiene missing values asi que vamos a averiguar cuantos missing values son y en base a eso podriamos tener una idea que medida tomar.

### Averiguando el impacto del missing value


```python
dataset['Class'].value_counts()
```




    2    458
    4    241
    Name: Class, dtype: int64




```python
missing_values['Class'].value_counts()
```




    2    14
    4     2
    Name: Class, dtype: int64


```python
(100*14)/458
```




    3.056768558951965




```python
(100*2)/241
```




    0.8298755186721992


Para los casos benignos (458) tenemos un `3.056%` de missing values mientras que para los casos malignos (241) tenemos un total de `0.829%` missing values.

```python
dataset = dataset.drop(dataset.index[missing_values.index])
```
### Outlyers

Para poder analizar los outliers vamos a utilizar diagramas de caja, una tecnica muy popular para analizar variables univariantes.

```python
fig, ax = plt.subplots(figsize=(12,8))
sns.boxplot(ax=ax, data=dataset.iloc[:, : 5])
```


![png](./1_src/img/output_13_1.png)



```python
fig, ax = plt.subplots(figsize=(12,8))
sns.boxplot(ax=ax, data=dataset.iloc[:, 5:9])
```


![png](./1_src/img/output_14_1.png)

Notar que no se realizo un diagrama para la variable de salida ya que en este caso son dos salidas y el diagrama no aportaria informacion relevante.


Vamos a analizar los outliers detectados


```python
marginal_adhesion_outlyers = dataset[dataset['Marginal Adhesion']>9]
single_ephithelial_cell_size_outlyers = dataset[dataset['Single Epithelial Cell Size']>8]
bland_chromatin_outlyers = dataset[dataset['Bland Chromatin']>9]
normal_nucleoli_outlyers = dataset[dataset['Normal Nucleoli']>8]
mitosis_outlyers = dataset[dataset['Mitoses']>1]
```


```python
print(
    (len(marginal_adhesion_outlyers)*100)/len(dataset),
    (len(single_ephithelial_cell_size_outlyers)*100)/len(dataset),
    (len(bland_chromatin_outlyers)*100)/len(dataset),
    (len(normal_nucleoli_outlyers)*100)/len(dataset),
    (len(mitosis_outlyers)*100)/len(dataset))
```

    8.052708638360176 4.831625183016105 2.9282576866764276 10.980966325036603 17.569546120058565


|               | Marginal Adhesion | Single Epithelial Cell Size |  Bland Chromatin | Normal Nucleoli | Mitoses |
|---------------|:-----------------:|:---------------------------:|:----------------:|:---------------:|:-------:|
|**Porcentaje** | 8.05 %           | 4.31 %                      | 2.92 %          | 10.98 %        | 17.56 % |
|**Valores > a**| 9                 | 8                           | 9                | 8               | 1       |

### Correlacion en los datos


```python
data_correlation = dataset.corr()
mask = np.zeros_like(data_correlation, dtype=np.bool)
mask[np.triu_indices_from(mask)] = True
f, ax = plt.subplots(figsize=(11, 9))
cmap = sns.diverging_palette(220, 10, as_cmap=True)
sns.heatmap(data_correlation, mask=mask, cmap=cmap,  center=0,
            square=True, linewidths=.5)
```



![png](./1_src/img/output_20_1.png)

```python
data_correlation['Class'].sort_values(ascending=False)
```


    Class                          1.000000
    Uniformity of Cell Shape       0.821891
    Uniformity of Cell Size        0.820801
    Bland Chromatin                0.758228
    Normal Nucleoli                0.718677
    Clump Thickness                0.714790
    Marginal Adhesion              0.706294
    Single Epithelial Cell Size    0.690958
    Mitoses                        0.423448
    Name: Class, dtype: float64



Podemos ver que "Uniformity of Cell Shape" y "Uniformity of Cell Size" estan muy relacionadas con la clase de tumor.

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


## CART 



```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib
from sklearn import tree
import graphviz
```


```python
data = pd.read_csv('breast_cancer.csv')
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
      <th>id number</th>
      <th>Clump Thickness</th>
      <th>Uniformity of Cell Size</th>
      <th>Uniformity of Cell Shape</th>
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




```python
data = data.replace('?', np.nan)
```


```python
data = data.dropna(axis=0)
```


```python
data = data.drop('id number', axis=1)
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
      <th>Clump Thickness</th>
      <th>Uniformity of Cell Size</th>
      <th>Uniformity of Cell Shape</th>
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




```python
data_target = pd.DataFrame(data['Class'])
data = data.drop('Class', axis=1)
```


```python
data_target.head(2)
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




```python
data.head(2)
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
      <th>Uniformity of Cell Shape</th>
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
clf = tree.DecisionTreeClassifier()
clf = clf.fit(data, data_target)
```


```python
dot_data = tree.export_graphviz(clf, out_file=None) 
graph = graphviz.Source(dot_data) 
graph.render('Breast Cancer') 
```




    'Breast Cancer.pdf'




```python
dot_data = tree.export_graphviz(clf, out_file=None, 
                         feature_names=['Clump Thickness', 'Uniformity of Cell Size',
                                        'Uniformity of Cell Shape', 'Marginal Adhesion',
                                        'Single Epithelial Cell Size', 'Bare Nuclei', 'Bland Chromatin',
                                        'Normal Nucleoli', 'Mitoses'],  
                         class_names=['0','2'],  
                         filled=True, rounded=True,  
                         special_characters=True)  
graph = graphviz.Source(dot_data)  
graph 
```




![svg](output_12_0.svg)



Modelo muy complejo



![CART](./1_src/output_12_0.svg)

<img src="./1_src/output_12_0.svg">