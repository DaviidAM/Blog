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

# Es bueno limpiar y ordenas los datos como mejor veamos conveniente.
df = df.drop('id', axis=1) # Hay columnas como el id que posiblemente no nos sirva, por lo que podemos eliminarlas.
df['date'] = pd.to_datetime(df['date']) # Para las fechas es importante pasarlo a un formato entendible por la red.
# Crear nuevas columnas con valores extraídos de la fecha.
df['year'] = df['date'].apply(lambda date: date.year)
df['month'] = df['date'].apply(lambda date: date.month)

# Es importante ver cómo se correlacionan (como influyen unos valores sobre otros) dependiendo la categoría
# Ejemplo: Precio debe tener una alta correlación con los metros cuadrados
df.corr()['price'].sort_values()
'''
zipcode         -0.053402
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

# Se pueden agrupar valores
df.groupby('month').mean()['price'].plot()

# Ver valores que se repiten
df['zipcode'].value_counts()

```

## Preprocesamiento

Continuando el ejemplo anterior donde trabajábamos con características de casas.

```python
# Extraemos datos de entrada y salida del modelo
X = df.drop('price', axis = 1).values
# Usamos .values para devolver numpy array en vez de Dataframe
y = df['price'].values

from sklearn.model_selection import train_test_split

# Separar entre pruebas de testeo y muestras de entrenamiento
X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.3,random_state=101)

# Escalar datos
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
X_train= scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

## Trabajar con el modelo

Continuando el ejemplo anterior donde trabajábamos con características de casas.

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Activation
from tensorflow.keras.optimizers import Adam

# Crear modelo

# X_train.shape
# Salida: (15117, 19)
# Al tener 19 características es buena idea tener 19 neuronas por capa.


model = Sequential()

model.add(Dense(19,activation='relu'))
model.add(Dense(19,activation='relu'))
model.add(Dense(19,activation='relu'))
model.add(Dense(19,activation='relu'))
model.add(Dense(1))

model.compile(optimizer='adam',loss='mse')

# Entrenar modelo
model.fit(x=X_train,y=y_train.values,
          validation_data=(X_test,y_test.values), # "validation_data" es para saber cómo va el porcentaje de acierto con los datos de testeo.
          batch_size=128, # Agrupar datos, normalmente se usa exponentes de 2.
          epochs=400) # Número de iteraciones

# Tabla con el histórico de las pérdidas del entrenamiento
losses = pd.DataFrame(model.history.history)
losses.plot()

# Calcular errores
from sklearn.metrics import mean_squared_error,mean_absolute_error,explained_variance_score

predictions = model.predict(X_test)

# Hay diferentes tipos de errores, dependiendo de los valores, es más útil uno u otro
mean_absolute_error(y_test,predictions)
np.sqrt(mean_squared_error(y_test,predictions))
explained_variance_score(y_test,predictions)

# Nuestras Predicciones
plt.scatter(y_test,predictions)
# Predicción perfecta
plt.plot(y_test,y_test,'r')
```

En este ejemplo había valores muy alejados (casas muy caras), posiblemente la mejor forma para tener un mejor resultado con nuestro modelo es eliminar ese porcentaje de casas excesivamente caras y volver a entrenar el modelo.
