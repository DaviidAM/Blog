# TensorBoard

TensorBoard es una herramienta esencial para el flujo de trabajo de aprendizaje automático, que ofrece mediciones y visualizaciones completas. Permite a los usuarios rastrear métricas de experimentos como la pérdida y la precisión, visualizar el gráfico del modelo, proyectar incrustaciones en espacios de menor dimensión y mucho más.

Tutorial oficial: <https://www.tensorflow.org/tensorboard/get_started>

```python
import pandas as pd
import numpy as np

## Cargar datos de ejemplo
df = pd.read_csv('../DATA/cancer_classification.csv')

## Dividir en datos de entrenamiento y testeo
from sklearn.model_selection import train_test_split

X = df.drop('benign_0__mal_1',axis=1).values
y = df['benign_0__mal_1'].values

X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.25,random_state=101)

## Escalar datos
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

scaler.fit(X_train)

X_train = scaler.transform(X_train)
X_test = scaler.transform(X_test)

## Crear modelo
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Activation,Dropout

# Esta vez importamos TensorBoard
from tensorflow.keras.callbacks import EarlyStopping,TensorBoard

early_stop = EarlyStopping(monitor='val_loss', mode='min', verbose=1, patience=25)

# Preparamos TensorBoard
log_directory = 'logs/fit'

# Opcional: ordenar carpetas por fecha
# timestamp = datetime.now().strftime("%Y-%m-%d--%H%M")
# log_directory = log_directory + '/' + timestamp

board = TensorBoard(log_dir=log_directory,histogram_freq=1,
    write_graph=True,
    write_images=True,
    update_freq='epoch',
    profile_batch=2,
    embeddings_freq=1)

# Generamos modelo
model = Sequential()
model.add(Dense(units=30,activation='relu'))
model.add(Dropout(0.5))
model.add(Dense(units=15,activation='relu'))
model.add(Dropout(0.5))
model.add(Dense(units=1,activation='sigmoid'))
model.compile(loss='binary_crossentropy', optimizer='adam')

## Entrenar modelo
model.fit(x=X_train, 
          y=y_train, 
          epochs=600,
          validation_data=(X_test, y_test), verbose=1,
          callbacks=[early_stop,board]
          )
```

Una vez completado el entrenamiento podremos lanzar la aplicación de TensorBoard desde una terminal con el siguiente comando, lo que habilitará toda la información en `http://localhost:6006`.

```bash
tensorboard --logdir logs/fit   
```
