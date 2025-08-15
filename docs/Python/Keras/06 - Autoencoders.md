# Autoencoders en Keras



[Teoría Autoencoders](../../Machine%20Learning/06-Autoencoders.md)

## Ejemplo de Autoencoders con Keras

### Preparación de datos

```python
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

from sklearn.datasets import make_blobs

# Esta nueva funcion sirve para crear datasets rápidamente.
## Centers = número de clusters.
## random_state = semilla para reproducibilidad.
data = make_blobs(n_samples=1000, n_features=2, centers=2, cluster_std=0.60, random_state=101)

X,y = data
# X = datos (valores de n_features)
# y = etiquetas (valores de centers/clusters), 0 o 1 en este ejemplo.

# Generamos nueva serie de valores que representan ruido
np.random.seed(seed=101)
z_noise = np.random.normal(size=len(X))
z_noise = pd.Series(z_noise)

# Añadimos el ruido a los datos en una nueva columna
feat = pd.DataFrame(X)
feat = pd.concat([feat, z_noise], axis=1)
feat.columns = ['X1', 'X2', 'X3']

plt.scatter(feat['X1'], feat['X2'], c=y)
plt.show()
# Aquí veremos que los datos están bien separados.

from mpl_toolkits.mplot3d import Axes3D

# Para crear una gráfica interactiva en notebook
# %matplotlib notebook

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.scatter(feat['X1'], feat['X2'], feat['X3'], c=y)
plt.show()
# Aquí veremos que los datos no están bien separados en el nuevo eje X3.
```

### Crear el modelo

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.optimizers import SGD

# Vamos a crear un autoencoder 3 -> 2 -> 3

# Creamos el encoder
encoder = Sequential()
encoder.add(Dense(units=2, input_shape=[3], activation='relu'))

# Creamos el decoder
decoder = Sequential()
decoder.add(Dense(units=3, input_shape=[2], activation='relu'))

# Creamos el autoencoder uniendo encoder y decoder
autoencoder = Sequential([encoder, decoder])

# lr = learning_rate
autoencoder.compile(optimizer=SGD(lr=1.5), loss='mse')
```

### Escalar los datos

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
scaled_data = scaler.fit_transform(feat)

```

### Predecir datos

```python
autoencoder.fit(scaled_data, scaled_data, epochs=5)

encoded_2dim = encoder.predict(scaled_data)

encoded_2dim.shape # (300, 2)
scaled_data.shape # (300, 3)

# Ver datos codificados
plt.scatter(encoded_2dim[:,0], encoded_2dim[:,1], c=y)
plt.show()
# Aquí veremos que los datos están bien separados.
```


