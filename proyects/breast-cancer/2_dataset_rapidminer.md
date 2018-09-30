# Analizando el Dataset

Primero importamos y cargamos los rapidamente los datos en RapidMiner.

![Dataset](./img/load_rm.png)

Renombramos el dataset

![Dataset](./img/rapidminer_load.png)

Ahora miramos las estadisticas para poder ver las distribuciones de los datos.

### Clump Thickness

![Dataset](./img/clump_thickness_h.png)

### Uniformity of Cell Size

![Dataset](./img/uniformity_of_cell_size.png)

### Uniformity of Cell Shape

![Dataset](./img/uniformity_of_cell_shape.png)

### Marginal Adhesion

![Dataset](./img/marginal_adhesion.png)

### Single Epithelial Cell

![Dataset](./img/single_epithelial_cell.png)

### Bare Nuclei

![Dataset](./img/bare_nuclei.png)

### Bland Chromatin

![Dataset](./img/bland_chromatin.png)

### Normal Nucleoli

![Dataset](./img/normal_nucleoli.png)

### Mitoses

![Dataset](./img/mitoses.png)

### Classes

![Dataset](./img/classes.png)


Podemos ver que casi todos los datos menos Clump Thickness tienen una distribucion similar (Similar a una beta).
Tambien podemos ver que no se indican missing values pero que `Bare Nuclei` es de tipo `Polynomial` cuando el dataset indicaba que todos los atributos eran numericos, vamos a tener que analizar esto como un posible caso de missing values.
