# Estudio de atributos

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

![Normal cell vs Cancer cell](./img/cell_structure.jpg)

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

[Dataset ➡](./2_dataset.md)