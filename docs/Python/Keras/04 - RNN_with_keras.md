# RNN en Keras

Como ya explicamos en la teoría de **redes neuronales relacionales (RNNs)**, estas redes neuronales son muy útil cuando necesitamos trabajar con secuencias de datos.

[Teoría RNNs](../../Machine%20Learning/Deep%20Learning/04-RNN.md)

## Ejemplo RNN con una onda sinusoidal

### Generar datos

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

# Generar datos
x = np.linspace(0, 50, 501) # 501 puntos entre 0 y 50
y = np.sin(x)

'''
# Mostrar Gráfica 
plt.plot(x, y)
plt.show()
'''

# Crear dataframe
df = pd.DataFrame(data=y, index=x, columns=['sin'])


# Separar datos de entrenamiento y test
test_percent = 0.1 # 10%
test_point = np.round(len(df) * test_percent) # 50 son los números de valores de test
test_ind = int(len(df) - test_point) # índice donde empieza el test (451)

train = df.iloc[:test_ind]
test = df.iloc[test_ind:]

# Escalar datos
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
scaler.fit(train)
scaled_train = scaler.transform(train)
scaled_test = scaler.transform(test)
```

### Crear lotes (batches) de datos

```python
from tensorflow.keras.preprocessing.sequence import TimeseriesGenerator

length = 50
batch_size = 1

# TimeseriesGenerator(x, y, length, batch_size) -> Genera lotes de datos de una secuencia
# x: Datos de entrada (En este caso scaled_train es un array de 2 dimensiones  que contiene x e y)
# y: Datos de salida (En este caso scaled_train es un array de 2 dimensiones  que contiene x e y)
# length: Número de pasos en el futuro que queremos predecir. Debe ser suficientemente grande o pequeño para predecir patrones en la serie de datos.
# batch_size: Número de lotes que queremos que se generen en cada iteración

generator = TimeseriesGenerator(scaled_train, scaled_train, length=length, batch_size=batch_size)

'''
Si tenemos una serie de datos que sea [1,2,3,4,5,6,7,8,9,10]
el generator generará los siguientes lotes:

Entrada: [(1,2), (2,3), (3,4), (4,5), (5,6), (6,7), (7,8), (8,9), (9,10)]
Salida: [3,4,5,6,7,8,9,10]

X,y = generator[0]
X = [1,2]  # 2 elementos porque length = 2
y = [3]    # 1 elemento porque batch_size = 1

X,y = generator[1]
X = [2,3]
y = [4]

(...)
'''
```

### Crear y entrenar el modelo

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, SimpleRNN, LSTM

n_features = 1 # x

model = Sequential()
#units: número de neuronas, debe ser igual al número de batches (length)
model.add(SimpleRNN(units=50, input_shape=(length, n_features)))
model.add(Dense(1))

# model.summary()

# Compilar modelo
model.compile(loss='mse', optimizer='adam')

# Entrenar modelo
model.fit_generator(generator, epochs=5, verbose=0)

# Ver error
losses=pd.DataFrame(model.history.history)
losses.plot()

# Predecir
first_eval_batch = scaled_train[-length:]
first_eval_batch = first_eval_batch.reshape((1, length, n_features))
y_pred = model.predict(first_eval_batch) # [0.948267] -> valor predicho

scaled_test[0] # [0.94955134] -> valor real
```

### Visualizar resultados

```python
test_predictions = []

first_eval_batch = scaled_train[-length:]
current_batch = first_eval_batch.reshape((1, length, n_features))

current_batch.shape # (1, 50, 1)

for i in range(len(test)):
    
    # obtener predicción 1 tiempo adelante ([0] es para obtener solo el número en lugar de [array])
    current_pred = model.predict(current_batch)[0]
    
    # almacenar predicción
    test_predictions.append(current_pred) 
    
    # actualizar lote para incluir predicción y eliminar el primer valor
    current_batch = np.append(current_batch[:,1:,:],[[current_pred]],axis=1)

# Como los valores están escalador, tenemos que invertir el escalado
true_predictions = scaler.inverse_transform(test_predictions)

# IGNORE WARNINGS
test['Predictions'] = true_predictions

test.plot(figsize=(12,8))
```

### Early Stopping

```python
from tensorflow.keras.callbacks import EarlyStopping

early_stop = EarlyStopping(monitor='val_loss',patience=2)

length = 49
batch_size = 1

generator = TimeseriesGenerator(scaled_train, scaled_train, length=length, batch_size=batch_size)
validation_generator = TimeseriesGenerator(scaled_test, scaled_test, length=length, batch_size=batch_size)

model = Sequential()
#units: número de neuronas, debe ser igual al número de batches (length)
model.add(LSTM(units=50, input_shape=(length, n_features)))
model.add(Dense(1))

# Compilar modelo
model.compile(loss='mse', optimizer='adam')

model.fit_generator(generator, epochs=20, verbose=0, validation_data=validation_generator, callbacks=[early_stop])
```

## Ejemplo RNN con series de tiempo

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

## Cargar datos
# Ejemplo de datos de precio dependiendo del día
df = pd.read_csv('RSCCASN.csv', parse_dates=True, index_col='DATE') 

# df.info()
# df.head()

df.columns = ['Precio']

df.plot(figsize=(12,8))
plt.show()  

## Preparar datos
# En este ejemplo los precios son por meses, la longitud total es 334 meses, vamos a coger los últimos 18 meses para el testeo

test_size = 18 # 18 meses
test_ind = int(len(df) - test_size)

train = df.iloc[:test_ind]
test = df.iloc[test_ind:]

## Escalar datos
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
scaler.fit(train)
scaled_train = scaler.transform(train)
scaled_test = scaler.transform(test)

## Crear lotes de datos
from tensorflow.keras.preprocessing.sequence import TimeseriesGenerator

# Como vamos a querer añadir un early stop es importante que en el generador la longitud sea menor que la lóngitud de los datos de testeo

length = 12 # 12 meses < test_size(18 meses)
batch_size = 1

# TimeseriesGenerator(x, y, length, batch_size) -> Genera lotes de datos de una secuencia
# x: Datos de entrada (En este caso scaled_train es un array de 2 dimensiones  que contiene x e y)
# y: Datos de salida (En este caso scaled_train es un array de 2 dimensiones  que contiene x e y)
# length: Número de pasos en el futuro que queremos predecir. Debe ser suficientemente grande o pequeño para predecir patrones en la serie de datos.
# batch_size: Número de lotes que queremos que se generen en cada iteración
generator = TimeseriesGenerator(scaled_train, scaled_train, length=length, batch_size=batch_size)

## Crear modelo
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, LSTM

n_features = 1 # x

model = Sequential()
#units: número de neuronas, debe ser igual al número de batches (length)
model.add(LSTM(units=100, activation='relu', input_shape=(length, n_features)))
model.add(Dense(1))
model.compile(loss='mse', optimizer='adam')

model.summary()

# early stop
from tensorflow.keras.callbacks import EarlyStopping

early_stop = EarlyStopping(monitor='val_loss', patience=2)

validation_generator = TimeseriesGenerator(scaled_test, scaled_test, length=length, batch_size=batch_size)

model.fit_generator(generator, epochs=20, verbose=0, validation_data=validation_generator, callbacks=[early_stop])

losses=pd.DataFrame(model.history.history)
losses.plot()

# Predecir valores de testeo
test_predictions = []

first_eval_batch = scaled_train[-length:]
current_batch = first_eval_batch.reshape((1, length, n_features))

for i in range(len(test)):
    
    # obtener predicción 1 tiempo adelante ([0] es para obtener solo el número en lugar de [array])
    current_pred = model.predict(current_batch)[0]
    
    # almacenar predicción
    test_predictions.append(current_pred) 
    
    # actualizar lote para incluir predicción y eliminar el primer valor
    current_batch = np.append(current_batch[:,1:,:],[[current_pred]],axis=1)

# Como los valores están escalador, tenemos que invertir el escalado
true_predictions = scaler.inverse_transform(test_predictions)

# IGNORE WARNINGS
test['Predictions'] = true_predictions

test.plot(figsize=(12,8))
```

### Predecir valores futuros (valores tras pruebas de testeo)

```python
full_scaler = MinMaxScaler()
scaled_full_data = full_scaler.fit_transform(df)

length = 12

full_generator = TimeseriesGenerator(scaled_full_data, scaled_full_data, length=length, batch_size=1)

model = Sequential()
#units: número de neuronas, debe ser igual al número de batches (length)
model.add(LSTM(units=100, activation='relu', input_shape=(length, n_features)))
model.add(Dense(1))
model.compile(loss='mse', optimizer='adam')

# no podemos usar un early stop porque no tenemos datos de validación, estamos prediciendo valores futuros
# epochs = 8 porque es un valor que en el caso anterior (cuando validábamos con los muestras de testeo) vimos que tenía poco error
model.fit_generator(full_generator, epochs=8)

forecast = []
periods = 12

first_eval_batch = scaled_full_data[-length:]
current_batch = first_eval_batch.reshape((1, length, n_features))

for i in range(periods):
    
    # obtener predicción 1 tiempo adelante ([0] es para obtener solo el número en lugar de [array])
    current_pred = model.predict(current_batch)[0]
    
    # almacenar predicción
    forecast.append(current_pred) 
    
    # actualizar lote para incluir predicción y eliminar el primer valor
    current_batch = np.append(current_batch[:,1:,:],[[current_pred]],axis=1)

# Como los valores están escalador, tenemos que invertir el escalado
forecast = full_scaler.inverse_transform(forecast)

# Calcular Date time de las predicciones
forecast_index = pd.date_range(start='2019-11-01', periods=periods, freq='MS') # 'MS' es Monthly Start
#forecast_index = pd.date_range(start=df.index[-1], periods=periods, freq='M') # Otra forma automática

forecast_df = pd.DataFrame(data=forecast, index=forecast_index, columns=['Forecast'])

# Mostrar gráfica de predicción
forecast_df.plot(figsize=(12,8))

# Mostrar gráfica de predicción + datos reales
ax = df.plot()
forecast_df.plot(ax=ax)

# Zoom in en la gráfica
plt.xlim('2018-01-01', '2020-12-01')
plt.show()  
```
