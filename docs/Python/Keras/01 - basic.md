# Keras

**Keras** es una biblioteca de Python utilizada como API para crear redes neuronales de **TensorFlow** fácilmente.

Keras ya viene instalado dentro de Tensorflow por lo que no hay que instalarla manualmente.

## Preparar los datos

```python
import pandas as pd
import numpy and np

import seaborn as sns # Para visualizar datos

# Leer tabla de un archivo csv
df = pd.read_csv('.../x.csv')

# Tabla con una lista de precios dependiendo de dos características.
df.head # Muestra una parte de la tabla

# Mostrar gráficos con los datos de cada columna de la tabla
sns.pairplot(df)
```

Es importante sacar una cantidad de valores de la tabla para entrenar y testear la red neuronal.

```python
from sklearn.model_selection import train_test_split

# Extraemos los datos de entrada
X = df[['feature1', 'feature2']].values
# ".value" porque por como funciona Tensorflow, el tipo tiene que ser numpy array
# "X" en mayus porque es un vector de varias dimensiones (n-dimensiones).

# Predicciones
y = df['price'].values
# ".value" porque por como funciona Tensorflow, el tipo tiene que ser numpy array
# "y" minus porque es un vector de una dimensión


# Separar datos entre entrenameinto y testeo
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
# test_size: Porcentaje de valores que van a ser para testeo
# random_state: Semilla que se usa para generar un valor random para coger los valores. Si se quiere coger los mismos grupos de datos en el futuro hay que mantener este número.

# Comprobar tamaño de los grupos
X_train.shape # (700, 2) --> 70% del total
X_test.shape # (300, 2) --> 30% del total
```

Es importante normalizar y esclar los datos para que la red neuronal puede trabajar sin problemas.

```python
from sklearn.preprocessing import MinMaxScaler

# Normalizamos los datos
scaler = MinMaxScaler() # Este scaler va a normalizar los datos entre 0 y 1.

# Calculamos cuales son los parámetros necesarios para normalizar los datos de entrenamiento.
# Esto se hace sólo en el conjunto de entrenamiento para no falsear el grupo de testeo.
scaler.fit(X_train) 
X_train = scaler.transform(X_train)
X_test = scaler.transform(X_test)
```

## Crear y entrenar modelo
