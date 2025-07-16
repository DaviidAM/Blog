# CNN en Keras

Como ya explicamos en la teoría de **redes neuronales convolucionales (CNNs)**, estas redes neuronales son muy útil cuando necesitamos trabajar con imágenes.

[Teoría CNNs](../../Machine%20Learning/03-CNN.md)

## CNN con imágenes en blanco y negro

Para este caso vamos a usar el dataset de MNIST, que contiene imágenes 28x28 en blanco y negro (1 canal de color) de números escritos a mano.

### Cargar datos

```python
import pandas as pd
import numpy as np

## Cargar datos de ejemplo
## MNIST es un dataset muy popular para entrenar con imágenes
from tensorflow.keras.datasets import mnist

(x_train, y_train), (x_test, y_test) = mnist.load_data()
```

### Comprobar dataset y visualizar imagen

```python
import matplotlib.pyplot as plt

x_train.shape # (60000, 28, 28) -> 60000 imágenes de 28x28 píxeles

single_image = x_train[0]
plt.imshow(single_image)
```

### Preprocesamiento de datos

```python
### En este caso queremos categorizar 10 valores (números del 0 al 9).
from tensorflow.keras.utils import to_categorical

y_cat_test = to_categorical(y_test,10)
y_cat_train = to_categorical(y_train,10)


### Hay que verificar que los valores estén normalizados entre 0 y 1,
### normalmente nos viene en valores entre 0 y 255
single_image.max() # 255
single_image.min() # 0

x_train = x_train/255
x_test = x_test/255

scaled_single = x_train[0]
scaled_single.max() # 1


### Reformatear canales
### En este ejemplo nos falta el canal del color en el formato
### (nº imágenes, ancho, alto, canales)
x_train.shape # (60000, 28, 28)

x_train = x_train.reshape(60000, 28, 28, 1)
```

### Preparar modelo

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Conv2D, MaxPool2D, Flatten

model = Sequential()

# La primera capa siempre suele ser una capa convolucional
# CONVOLUTIONAL LAYER
### filters: A mayor número de clasificaciones, mayor debe ser el número de
###     filtros, suele ser un exponente de 2.
### kernel_size: Tamaño de matriz donde vamos a aplicar el kernel/filtro.
###     Suele ser múltiplos de 2, empezar por (2,2) o (4,4) e ir subiendo.
### strides: Cuánto se mueve el kernel tras cada iteración. En este ejemplo
###     se usa valor por defecto (1,1). Para imágenes grandes es mejor aumentarlo.
### padding: Valor para decidir si queremos añadir padding o no. 
###     "valid" or "same" (default is "valid" == no padding).
### input_shape: Tamaño imagen.
model.add(Conv2D(filters=32, kernel_size=(4,4),input_shape=(28, 28, 1), activation='relu',))

# POOLING LAYER
## Normalmente el parámetro pool_size sigue la misma regla que kernel_size de la capa
##      convolucional.
model.add(MaxPool2D(pool_size=(2, 2)))

# FLATTEN IMAGES
# Aplanar imágenes de 28x28 a 764 antes de la capa final
model.add(Flatten())

# Dense layer para disminuir el número de neuronas, debe usarse un exponente de 2.
model.add(Dense(128, activation='relu'))

# La última capa es la que se usa para clasificar, debe haber una neurona por clase.
# En etse caso tenemos 10 clases posibles.
model.add(Dense(10, activation='softmax'))

model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy']) # Más métricas explicadas en https://keras.io/metrics/
```

### Entrenar modelo

```python
from tensorflow.keras.callbacks import EarlyStopping

early_stop = EarlyStopping(monitor='val_loss',patience=2)

model.fit(x_train,y_cat_train,epochs=10,validation_data=(x_test,y_cat_test),callbacks=[early_stop])
```

### Evaluar modelo

```python
# Mostrar métricas del entrenamiento
model.metrics_names # ['loss', 'accuracy']

losses = pd.DataFrame(model.history.history)
losses[['accuracy','val_accuracy']].plot()
losses[['loss','val_loss']].plot()

model.evaluate(x_test,y_cat_test,verbose=0) # [0.04325260493150563, 0.9867]

# Reporte de la classificación y matriz de confusión
from sklearn.metrics import classification_report,confusion_matrix

predictions = model.predict_classes(x_test)
predictions[0] #7
print(classification_report(y_test,predictions))
'''
precision    recall  f1-score   support

           0       0.99      0.99      0.99       980
           1       0.99      1.00      0.99      1135
           2       0.99      0.98      0.98      1032
           3       0.99      0.99      0.99      1010
           4       0.97      1.00      0.98       982
           5       0.99      0.99      0.99       892
           6       1.00      0.98      0.99       958
           7       0.97      1.00      0.98      1028
           8       0.99      0.98      0.98       974
           9       0.99      0.96      0.98      1009

    accuracy                           0.99     10000
   macro avg       0.99      0.99      0.99     10000
weighted avg       0.99      0.99      0.99     10000
'''

confusion_matrix(y_test,predictions)
'''
array([[ 975,    0,    1,    0,    0,    0,    3,    1,    0,    0],
       [   0, 1134,    0,    0,    0,    0,    0,    1,    0,    0],
       [   1,    4, 1007,    1,    4,    0,    0,   13,    2,    0],
       [   0,    0,    5, 1000,    0,    2,    0,    2,    1,    0],
       [   0,    0,    0,    0,  979,    0,    0,    0,    0,    3],
       [   0,    0,    0,    6,    0,  883,    1,    0,    2,    0],
       [   4,    3,    0,    1,    5,    1,  943,    0,    1,    0],
       [   0,    1,    2,    0,    0,    0,    0, 1024,    1,    0],
       [   2,    1,    4,    4,    4,    0,    0,    5,  952,    2],
       [   0,    3,    0,    1,   16,    4,    0,   14,    1,  970]],
      dtype=int64)
'''
# visualizar confusion matrix
import seaborn as sns
plt.figure(figsize=(10,6))
sns.heatmap(confusion_matrix(y_test,predictions),annot=True)

# Usar modelo con una nueva imagen
my_number = x_test[0]
plt.imshow(my_number.reshape(28,28)) # Comprobar que es un siete

## Recordar hacer reshape --> (num_images,width,height,color_channels)
model.predict_classes(my_number.reshape(1,28,28,1)) # array([7], dtype=int64)
```

## CNN con imágenes a color

Para este caso vamos a usar el dataset de CIFAR-10, que contiene imágenes 32x32 a color (3 canales de colores - RGB) de 10 clases de objetos diferentes como aviones, gatos, perros, etc.

La forma de trabajar con la red neuronal desde keras es 90% idéntica al ejemplo anterior cuando trabajábamos con imágenes en blanco y negro (1 canal de color).

```python
import pandas as pd
import numpy as np

# Cargar datos
from tensorflow.keras.datasets import cifar10

(x_train, y_train), (x_test, y_test) = cifar10.load_data()

x_train.shape # (50000, 32, 32, 3) = 50000 imágenes de 32x32 con 3 canales de colores
x_train[0].shape # (32, 32, 3)


# Normalizar imágenes (dividir entre 255 cada pixel) es igual que antes
# (...)


# Crear categorías es igual que antes
# Nota: En este caso la categoría es sólo un número, por lo que tendremos que ir a la web oficial para ver qué objeto significa cada número
# (...)


# Preparar modelo
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Conv2D, MaxPool2D, Flatten

model = Sequential()

# En este caso input_shape es diferente porque el tamaño de la imagen es otro.
model.add(Conv2D(filters=32, kernel_size=(4,4),input_shape=(32, 32, 3), activation='relu',))

# POOLING LAYER
## Normalmente el parámetro pool_size sigue la misma regla que kernel_size de la capa
##      convolucional.
model.add(MaxPool2D(pool_size=(2, 2)))

# NOTA: como en este caso tenemos más valores que ajustar (32x32x3 = 3072) es recomendable
# añadir más capas convolucionales y pool layers. Al añadir más capas es recomendable cambiar
# el valor de filters, pero en este ejemplo vamos a dejar el mismo.
model.add(Conv2D(filters=32, kernel_size=(4,4),input_shape=(32, 32, 3), activation='relu',))

model.add(MaxPool2D(pool_size=(2, 2)))


# FLATTEN IMAGES
# Aplanar imágenes de 28x28 a 764 antes de la capa final
model.add(Flatten())

# Dense layer para disminuir el número de neuronas, debe usarse un exponente de 2.
model.add(Dense(128, activation='relu'))

# La última capa es la que se usa para clasificar, debe haber una neurona por clase.
# En etse caso tenemos 10 clases posibles.
model.add(Dense(10, activation='softmax'))

model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy']) # Más métricas explicadas en https://keras.io/metrics/

# Evaluar modelo
metrics = model.evaluate(x_test, y_cat_test)
metrics.columns

metrics[['accuracy','val_accuracy']].plot()
metrics[['loss','val_loss']].plot()

from sklearn.metrics import classification_report,confusion_matrix

predictions = model.predict_classes(x_test)
predictions[0] #7
print(classification_report(y_test,predictions))

confusion_matrix(y_test,predictions)

import seaborn as sns
plt.figure(figsize=(10,6))
sns.heatmap(confusion_matrix(y_test,predictions),annot=True)

# Usar modelo con una nueva imagen
my_image = x_test[0]
plt.imshow(my_image) 
## Recordar hacer reshape --> (num_images,width,height,color_channels)
model.predict_classes(my_image.reshape(1,32,32,3)) # array([3], dtype=int64)
```

## Otras funciones para CNN

### Generar datos de imágenes

Existe la clase `ImageDataGenerator` que permite generar datos de imágenes de manera dinámica, lo que permite aumentar el tamaño del dataset y ahorrar memoria al no tener que cargar todas las imágenes en memoria.

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

image_gen = ImageDataGenerator(
    rotation_range=10, # gira las imágenes
    width_shift_range=0.1, # desplaza las imágenes horizontalmente
    height_shift_range=0.1, # desplaza las imágenes verticalmente
    shear_range=0.1, # distorsiona las imágenes
    zoom_range=0.1, # zoom en las imágenes
    horizontal_flip=True, # invierte las imágenes
    fill_mode='nearest' # rellena las imágenes
)
image_gen.random_transform(x_train[0]) # Aplica de forma random las transformaciones sobre la imagen seleccionada.

image_gen.flow_from_directory(train_path) # Genera datos de imágenes de manera dinámica. Para que funcione correctamente, el dataset debe estar organizado en carpetas con el nombre de la categoría.
# Ejemplo:
# train_path
#     ├── cat
#     │   ├── cat.0.jpg
#     │   ├── cat.1.jpg
#     │   ├── cat.2.jpg
#     │   ├── cat.3.jpg
#     │   ├── cat.4.jpg
#     │   ├── cat.5.jpg
#     │   ├── cat.6.jpg
#     │   ├── cat.7.jpg
#     │   ├── cat.8.jpg
#     │   └── cat.9.jpg
#     └── dog
#         ├── dog.0.jpg
#         ├── dog.1.jpg
#         ├── dog.2.jpg
#         ├── dog.3.jpg
#         ├── dog.4.jpg
#         ├── dog.5.jpg
#         ├── dog.6.jpg
#         ├── dog.7.jpg
#         ├── dog.8.jpg
#         └── dog.9.jpg

# Usar ImageDataGenerator para generar datos de pruebas y test de manera dinámica.
train_image_gen = image_gen.flow_from_directory(
    train_path,
    target_size=image_shape[:2], # Tamaño de la imagen (ancho, alto, canales)
    batch_size=16,
    class_mode='binary', # Clasificación binaria (otra opcion es 'categorical')
    color_mode='rgb'
)
test_image_gen = image_gen.flow_from_directory(
    test_path,
    target_size=image_shape[:2],
    batch_size=16,
    class_mode='binary',
    color_mode='rgb',
    shuffle=False # No se debe mezclar el test set
)

# Comprobar categorías del train set
test_image_gen.class_indices # {'cat': 0, 'dog': 1}
```
