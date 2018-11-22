# Outliers

Podemos ver que la diferencia de tamaño es muy poca entre los femurs


```python
full_values_femur.describe()
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
      <th>Difference</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>5.00000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.34000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.74027</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.80000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.90000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.00000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.40000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.60000</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(2,8))
sns.boxplot(ax=ax, data=full_values_femur)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c7de76f4e0>




![png](./img/output_23_1.png)


Podemos ver que la diferencia entre el femur derecho e izquierdo  suele ser muy pequeña, asi que vamos a asignarle a los valores faltantes que tienen una contraparte su valor.


```python
data.loc[data['Femur left'].isnull(),'Femur left']   = data['Femur right']
data.loc[data['Femur right'].isnull(),'Femur right'] = data['Femur left']
```


```python
both_missing = data[data['Femur left'].isnull()==True]
len(both_missing)
```




    26



Podemos ver que quedamos con 26 de 117 valores NaN para los cuales no sabemos las medidas de ningun femur.


```python
both_missing
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
      <th>ID</th>
      <th>Location</th>
      <th>Site_Number</th>
      <th>Age_Minumum</th>
      <th>Age_Maximum</th>
      <th>Sex</th>
      <th>Grave Number</th>
      <th>Canine number</th>
      <th>Canine largest age</th>
      <th>Canine 2nd largest age</th>
      <th>...</th>
      <th>Height in grave</th>
      <th>Abnormalities Vertebras</th>
      <th>Femur left</th>
      <th>Femur right</th>
      <th>Abnormalities Femur</th>
      <th>Notes</th>
      <th>Date</th>
      <th>Signature</th>
      <th>Hyperplasia</th>
      <th>Teeth Scorable</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>9.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>50.0</td>
      <td>60.0</td>
      <td>Male</td>
      <td>G377</td>
      <td>3.0</td>
      <td>3.5</td>
      <td>5.0</td>
      <td>...</td>
      <td>166.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Kn?kkede postmortalt s? kan ikke m?les</td>
      <td>Incisor mere end 1/3 slidt ned</td>
      <td>5/8/2008</td>
      <td>MWOD</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>12.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>50.0</td>
      <td>60.0</td>
      <td>Female</td>
      <td>G104</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>150.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Kraniet mangler, kig om det er p? udstilling</td>
      <td>5/8/2008</td>
      <td>MWOD</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>13.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>35.0</td>
      <td>44.0</td>
      <td>Female</td>
      <td>G159</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>4.3</td>
      <td>...</td>
      <td>158.5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Begge l?rben for nedbrudte til at m?le</td>
      <td>NaN</td>
      <td>5/8/2008</td>
      <td>MWOD</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>46</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>52</th>
      <td>56.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>35.0</td>
      <td>40.0</td>
      <td>Female</td>
      <td>G808</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>153.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Intet at kigge p?. Intet kranie. Female?</td>
      <td>5/27/2008</td>
      <td>MWOD</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>53</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>54</th>
      <td>58.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>50.0</td>
      <td>60.0</td>
      <td>NaN</td>
      <td>G802</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>2.5</td>
      <td>...</td>
      <td>165.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>K?n ukendt. T?nder har ikke noget at sidde fas...</td>
      <td>5/27/2008</td>
      <td>MWOD</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>55</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>56</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>68</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>69</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>70</th>
      <td>74.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>40.0</td>
      <td>50.0</td>
      <td>Male</td>
      <td>G846</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>173.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>T?nder faldet ud og remodeleret. Begge l?rben ...</td>
      <td>5/27/2008</td>
      <td>MWOD</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>72</th>
      <td>76.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>30.0</td>
      <td>40.0</td>
      <td>Male</td>
      <td>G897</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>162.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5/28/2008</td>
      <td>MWOD</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>74</th>
      <td>78.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>35.0</td>
      <td>45.0</td>
      <td>Male</td>
      <td>G894</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>168.5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Der er et s?t l?rben i overskud  og jeg m?ler ...</td>
      <td>5/28/2008</td>
      <td>MWOD</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>76</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>78</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>83</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>84</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>85</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>86</th>
      <td>90.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>20.0</td>
      <td>25.0</td>
      <td>Female</td>
      <td>G99</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>161.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5/28/2008</td>
      <td>MWOD</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>91</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>92</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>94</th>
      <td>98.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>60.0</td>
      <td>NaN</td>
      <td>Male</td>
      <td>G419</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>167.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>T?nder mere end 1/3 slidte. Skelet i d?rlig st...</td>
      <td>5/28/2008</td>
      <td>MWOD</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>108</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>109</th>
      <td>113.0</td>
      <td>Ribe</td>
      <td>ASR1015</td>
      <td>25.0</td>
      <td>35.0</td>
      <td>Male</td>
      <td>G211</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.5</td>
      <td>...</td>
      <td>163.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Male?</td>
      <td>5/28/2008</td>
      <td>MWOD</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>26 rows × 23 columns</p>
</div>



Tambien podemos ver que la mayoria de estos casos no esta bien documentado y tiene muchos valores `NaN`. Vamos a eliminar los que tienen valores `NaN` en la altura de la tumba ya que no tienen informacion relevante para el caso de estudio.


```python
data = data[np.isfinite(data['Femur left'])]
```