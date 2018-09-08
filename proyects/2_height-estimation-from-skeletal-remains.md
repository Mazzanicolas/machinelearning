# Height Estimation from Skeletal Remains 
 
![Height Estimation from Skeletal
Remains](./2_img/banner.jpg) 

## Overview

## Dataset

### Cargando el dataset

El dataset se encuentra en un archivo `.accdb` (Microsoft Access) asi que lo primero que vamos a hacer es exportaro como `.csv`

![Acces DB](./2_img/accesdb.png)

Analizandolo rapidamente en un editor de texto podemos ver que contiene carateres que no van a poder ser decodeados con UTF-8 (`�`). 

![UTF-8 Error](./2_img/utf8Error.png)

Ahora ya podemos cargarlo con `pandas`

<object data="./2_src/pf_overview.html"> 
    Your browser doesn’t support the object tag. 
</object>