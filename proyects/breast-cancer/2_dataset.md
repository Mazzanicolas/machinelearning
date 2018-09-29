# Dataset

El dataset a ser estudiado esta compuesto por los resultados obtenidos mediante biopsias de ​FNA​ (fine needle aspiration), contiene 699 instancias con 10 atributos y una clase de salida (benigno o malicioso).
En una biopsia FNA, el doctor usa una aguja muy fina y hueca con una jeringa para poder aspirar una pequeña cantidad de tejido o fluido de un area sospechosa. La muestra recolectada es revisada para ver si hay celulas cancerígenas.
![FNA](./img/fna.gif)

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

## Problema

El problema consiste en predecir si una muestra de células recolectada mediante ​FNA​ es benigna o no en base a una serie de atributos.

[Attributes Text ➡](./proyects/breast-cancer/3_attributes.md)

[Attributes Code ➡](./proyects/breast-cancer/3_attributes.md)

[Attributes RapidMiner ➡](./proyects/breast-cancer/3_attributes.md)