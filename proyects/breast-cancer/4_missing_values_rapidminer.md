# Missing Values (RapidMiner)

Eliminamos la columna id y nos ponemos a analizar los missing values.

![Dataset](./img/rapidminer_missing_1.png)

![Dataset](./img/rapidminer_missing_2.png)

Podemos ver que en `Bare Nuclei` tenemos valores de `?`, RapidMiner no los clasifica como missing values porque interpreta `?` como un valor posible del atributo. Procedemos a eliminar las filas con esos missing values ya que no tenemos como estimarlos y son solo 16 casos en 699.

![Dataset](./img/rapidminer_missing_3.png)