# Regresión

## Analizar los datos

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Leer archivo csv - Valor de una casa dependiendo de sus características
df = pd.read_csv('../data/kc_house_data.csv')

# Ver si hay algún valor nulo
df.isnull().sum()

# Análisis estadístico de los datos
df.describe().transpose()

# Gráfica para ver el conteo del número de habitaciones
sns.countplot(x='bedrooms', data=df)

# Es importante ver cómo se correlacionan (como influyen unos valores sobre otros) dependiendo la categoría
# Ejemplo: Precio debe tener una alta correlación con los metros cuadrados
df = df.drop('date', axis=1) # Hay que eliminar la columna date porque si no la correlación no funciona.
df.corr()['price'].sort_values()
'''
zipcode         -0.053402
id              -0.016772
long             0.022036
condition        0.036056
yr_built         0.053953
sqft_lot15       0.082845
sqft_lot         0.089876
yr_renovated     0.126424
floors           0.256804
waterfront       0.266398
lat              0.306692
bedrooms         0.308787
sqft_basement    0.323799
view             0.397370
bathrooms        0.525906
sqft_living15    0.585241
sqft_above       0.605368
grade            0.667951
sqft_living      0.701917  <<<< Alta correlación
price            1.000000  <<<< Referencia
Name: price, dtype: float64
'''

# Mostrar ambos datos con alta correlación para ver que es bastabte lineal
plt.figure(figsize=(12,8))
sns.scatterplot(x='price',y='sqft_living',data=df)

# Mostrar distribución de precios por habitaciones
sns.boxplot(x='bedrooms',y='price',data=df)

# Gráfica de precios por ubicación
plt.figure(figsize=(12,8))
sns.scatterplot(x='long',y='lat',data=df,hue='price')
# Nota: Hay herramientas externas que te muestran estos valores sobre un mapa real.

# Hay veces en las que merece la pena quitar valores extremos, para tener un resultado más real
len(df)*(0.01) # Salida: 215.97 >> Equivale al 1% de los valores de la tabla
# Ordenamos y quitamos el 1% de las casas más caras
non_top_1_perc = df.sort_values('price',ascending=False).iloc[216:]
# Mostrar datos
plt.figure(figsize=(12,8))
sns.scatterplot(x='long',y='lat',
                data=non_top_1_perc,hue='price',
                palette='RdYlGn',edgecolor=None,alpha=0.2)


```
