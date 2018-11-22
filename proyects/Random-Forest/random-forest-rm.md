
# Random Forest

Para este algoritmo vamos a utilizar el dataset de Breast Cancer. El dataset contiene 32 atributos y una clase de salida B o M para identificar si es benigno o maligno.

Para random forest los datos no requieren nigun tipo de preparación de los datos.

Comenzamos importando el dataset a rapidminer, eliminamos las columnas inecesarias como, por ejemplo, id que no aporta informacion relevante y solo genera rudio y seleccionamos la clase de salida M/B como label a predecir.

Agregamos un modulo de cross validation:

![](./img/process1.PNG)

Dentro de este en el lado izquierdo agregamos el algoritmo de random forest con sus parametros por defectoy del lado derecho un apply model y performance.

![](./img/process2.PNG)

Podemos ver que tenemos una accuracy de 94.91 % con una class recall de 90 para M y 97.75% para B. Un modelo bastante bueno.

![](./img/capture.PNG)