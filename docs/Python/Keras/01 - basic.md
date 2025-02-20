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

Crear el modelo usando keras que se encuentra dentro de la librería de tensorflow.

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

# Creamos el modelo pasándole una lista de las capas que queremos tener.
# Dense layer es una capa que cada neurona conecta con cada neurona de la siguiente capa.
# units: número de neuronas por capa (4 en este ejemplo)
# activation function: relu
model = Sequential([Dense(units=4, activation='relu')])
# si queremos crear un modelo con dos capas, simplemente añadimos dos capas Dense a la lista.
model = Sequential([Dense(units=4, activation='relu'),
                    Dense(units=2, activation='relu'),
                    Dense(units=1)]) # Última capa no necesita función de activación

# Otra forma de crear un modelo
model = Sequential()
model.add(Dense(units=4, activation='relu'))
model.add(Dense(units=4, activation='relu'))
model.add(Dense(units=4, activation='relu'))
model.add(Dense(units=4, activation='relu'))
model.add(Dense(units=1)) # Sólo un nodo en la capa final porque sólo queremos un output

# Compilar el modelo
model.compile(
        optimizer='rmsprop', # modelo para calcular el gradiente descendiente
        loss='mse', # depende de qué queramos conseguir: multi-class clasification, binary classification or regression problem // See docstring from compiler function.      
)

# Entrenar modelo
model.fit(
    x=X_train, # Datos de entrada
    y=y_train, # Salida esperada
    epochs=250 # Cuantas veces se va a usar cada dato de entrada para entrenar el modelo
)

# Mostrar como el error ha ido disminuyendo
loss_df = pd.DataFrame(model.history.history)
loss_df.plot()
```

## Evaluar el modelo

El siguiente paso es testear el modelo con pruebas que nunca ha visto antes, es decir, las muestras de testeo.

```python
# Evaluar modelo entrenado
model.evaluate(X_test, y_test, verbose=0)
# Salida: 24.9722... Significa la pérdida, el error cuadrático medio (mse).

# Precedir salidas
test_predictions = model.predict(X_test)

# Preparar datos para crear gráfica entre valores reales y predicciones
test_predictions = pd.Series(test_predictions.reshape(300,))
pred_df = pd.DataFrame(y_test, columns['Test True Y'])
pred_df = pd.concat([pred_df, test_predictions], axis=1)
pred_df.columns = ['Test True Y', 'Model Predictions']

sns.scatterplot(x='Test True Y', y='Model Predictions', data=pred_df)
```

Podemos calcular rápidamente otros tipos de errores:

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error

mean_absolute_error(pred_df['Test True Y'], pred_df['Model Predictions'])
# Salida: aprox 4

df.describe()
# mean is aprox 500, so having an error of 4 means we model is working fine

mean_squared_error(pred_df['Test True Y'], pred_df['Model Predictions'])
# Salida: aprox 24.97...
```

Si quisiesemos probar con datos de entrada totalmente nuevos:

```python
# Nuevos datos de entrada
new_gem = [[998, 1000]]
# Es importante recordar que tenemos que escalar los datos de entrada
new_gem = scaler.transform(new_gem)
# Predecir usando el modelo
model.predict(new_gem)
# Salida: aprox 419
```

## Guardar y cargar modelos

Si entrenamos un modelo muy grande que requiere mucho tiempo de entrenamiento es importante guardar el modelo para no tener que entrenarlo cada vez que queramos usarlo.

```python
from tensorflow.keras.models import load_model

# Guardar modelo en archivo 'my_gem_model.h5' 
model.save('my_gem_model.h5')

# Cargar modelo
later_model = load_model('my_gem_model.h5')
later_model.predict(new_gem)
```
