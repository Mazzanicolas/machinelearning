# Height Estimation from Skeletal Remains 
 
![Height Estimation from Skeletal
Remains](./2_src/img/banner.jpg) 

El objetivo de este proyecto es poder estimar la altura de un ser humano en base a los restos óseos. Para esto vamos a utilizar el dataset utilizado en un  proyecto de biomedicina en la University of Southern Denmark, titulado “[Heigh Estimation from Skeletal Remains](http://www.adbou.dk/fileadmin/adbou/projektopgaver/ADBOU_linear_regression_Mette_Wodx.pdf)” el cual contiene información de medidas de diferentes restos óseos antiguos encontrados en el pueblo Ribe de Dinamarca.

## Introducción

Utilizar restos óseos para estimar la altura de una persona es una herramienta muy util en la medicina forense y antropología biológica. Muchas veces solo unos pocos huesos se encuentran en excavaciones arqueológicas. Pero con unos pocos huesos podemos estimar la altura de una persona, esto es posible por la relación de tamaño entre los huesos.

## Problema

 Se ha determinado que existe una relación entre el fémur, tibia, humero y el radio con la altura de una persona. A continuación se muestran las imágenes de los huesos mencionados para una referencia visual.

| Fémur | Tibia |
| :---: | :---: |
|![Fémur](./2_src/img/femur.gif)|![Tibia](./2_src/img/tibia.gif)|

| Humero | Radio |
| :---: | :---: |
|![Humero](./2_src/img/humerus.gif)|![Radio](./2_src/img/radius.gif)|

## Estudio de atributos

El dataset cuenta con 23 atributos:

* ID `Int64`
* Location `?`
* Site_Number `?`
* Age_Minumum `Int64`
* Age_Maximum `Float64`
* Sex `?`
* Grave Number `?`
* Canine number `Float64`
* Canine largest age `Float64`
* Canine 2nd largest age `Float64`
* Incisor number `Float64`
* Incisor largest age `Float64`
* Incisor 2nd largest age `Float64`
* Height in grave `Float64`
* Abnormalities Vertebras `?`
* Femur left `Float64`
* Femur right `Float64`
* Abnormalities Femur `?`
* Notes `?`
* Date `?`
* Signature `?`
* Hyperplasia `Bool`
* Teeth Scorable `Bool`

Estamos frente a un problema de regresión ya que la salida es un numero real. La variable objetivo es la altura de la persona, en este caso "Height in grave"

A continuación una breve descripción de las definiciones  para los atributos del dataset y porque pueden ser relevantes.

A simple vista podemos descartar el atributo id ya que no aporta valor y causa ruido.

**Location:** `Ubicación` *Este atributo toma dos valores `Ribe` y `G216`.*

Varias excavaciones y sitios se les ha dado diferentes nombres en el transcurso del tiempo (por ejemplo, Tirup es el mismo lugar que Bygholm) por lo cual tenemos que tener ciertas consideraciones con los datos. Aunque en este caso solo contamos contamos con un sitio, como veremos mas adelante.

`Ribe` es un puebo de Dinamarca establecido en la primera década del siglo VIII y atestiguado por primera vez en un documento fechado en 854, Ribe es la ciudad más antigua existente en Dinamarca (y en Escandinavia). Recientes excavaciones arqueológicas realizadas entre 2008 y 2012 en han llevado al descubrimiento de entre 2.000 y 3.000 tumbas cristianas.

![Ribe Town](./2_src/img/ribe.jpg)

`G216` es un valor desconocido.

####  Ocurrencias de los atributos

| Ribe | G216 |
|:----:|:----:|
| 116  | 1    |

Podemos ver que G216 tiene una gran probabilidad de ser un error en la recolección de los datos y lo analizaremos mas adelante

**Site_Number:** `Numero del sitio` *Este atributo toma un solo valor `ASR1015`*

`ASR1015El` es el número de sitio en el cual la autoridad de excavación registro de la excavación real. Este atributo es relevante para usar cuando varios las excavaciones, dispersas en el tiempo, han tenido lugar en el mismo sitio.

####  Ocurrencias de los atributos

| ASR1015 |
|:-------:|
| 117     |

**Age_Minumum:** `Edad Mínimo` *Este atributo toma*


####  Ocurrencias de los atributos

**Age_Maximum:** `Edad Máximo` *Este atributo toma*

####  Ocurrencias de los atributos

**Sex** `Sexo` *Este atributo toma*

####  Ocurrencias de los atributos

**Grave Number:** `Numero de Tumba` *Este atributo toma*

####  Ocurrencias de los atributos

**Canine number:** `Float64` *Este atributo toma*

####  Ocurrencias de los atributos

**Canine largest age:** `Float64` *Este atributo toma*

**Canine 2nd largest age:** `Float64` *Este atributo toma*

**Incisor number:** `Float64` *Este atributo toma*

**Incisor largest age:** `Float64` *Este atributo toma*

**Incisor 2nd largest age:** `Float64` *Este atributo toma*

**Height in grave:** `Float64` *Este atributo toma*

**Abnormalities Vertebras:** `?` *Este atributo toma*

**Femur left:** `Float64` *Este atributo toma*

**Femur right:** `Float64` *Este atributo toma*

**Abnormalities Femur:** `?` *Este atributo toma*

**Notes:** `Notas` *Este atributo toma*

**Date:** `Fecha` *Este atributo toma*

**Signature:** `?` *Este atributo toma*

**Hyperplasia:** `Bool` *Este atributo toma*


### Analizando el Dataset (código)

El dataset se encuentra en un archivo `.accdb` (Microsoft Access) asi que lo primero que vamos a hacer es exportarlo como `.csv`

![Acces DB](./2_src/img/accesdb.png)

Analizándolo rápidamente en un editor de texto podemos ver que contiene caracteres que no van a poder ser decodeados con UTF-8 (`�`). Los remplazamos rápidamente por `?`.

![UTF-8 Error](./2_src/img/utf8Error.png)

Ahora ya podemos cargarlo con `pandas`

[Vista detallada del dataset ➡](./2_src/pf_overview.html)


### Importando librerias

```python
import missingno as msno
import pandas as pd
```
### Cargando el dataset

```python
data = pd.read_csv('../dataindsamling.csv')
```
### Missing Values

```python
msno.matrix(data)
```


![png](./2_src/img/output_2_1.png)



```python
msno.bar(data)
```


![png](./2_src/img/output_3_1.png)


# References
[ADBOU](http://www.adbou.dk/fileadmin/adbou/projektopgaver/ADBOU_linear_regression_Mette_Wodx.pdf)

[Human Osteological Methods](http://www.adbou.dk/fileadmin/adbou/manualer/humostman2015.pdf)