# Outlyers

Para poder analizar los outliers vamos a utilizar diagramas de caja, una tecnica muy popular para analizar variables univariantes.

```python
fig, ax = plt.subplots(figsize=(12,8))
sns.boxplot(ax=ax, data=dataset.iloc[:, : 5])
```


![png](./img/output_13_1.png)



```python
fig, ax = plt.subplots(figsize=(12,8))
sns.boxplot(ax=ax, data=dataset.iloc[:, 5:9])
```


![png](./img/output_14_1.png)

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


[Correlación ➡](./6_correlacion.md)