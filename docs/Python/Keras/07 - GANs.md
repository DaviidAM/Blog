# GANs en Keras

[Teoría GANs](../../Machine%20Learning/Deep%20Learning/07-GANs.md)

## Ejemplo de GANs con Keras

### Preparación de datos

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from tensorflow.keras.datasets import mnist

# Cargamos los datos - Este dataset contiene imágenes de dígitos escritos a mano.
(X_train, y_train), (X_test, y_test) = mnist.load_data()

# Filtramos solo los ceros
only_zeros = X_train[y_train == 0]

only_zeros.shape # (5923, 28, 28)
```

### Crear modelo

```python
import tensorflow as tf
from tensorflow.keras.layers import Dense, Reshape, Flatten
from tensorflow.keras.models import Sequential

# Creamos el discriminador
discriminator = Sequential()
discriminator.add(Flatten(input_shape=[28, 28]))
discriminator.add(Dense(150, activation='relu'))
discriminator.add(Dense(100, activation='relu'))
# Capa final - Una neurona que devuelve 0 o 1 (falso o verdadero)
discriminator.add(Dense(1, activation='sigmoid'))

discriminator.compile(loss='binary_crossentropy', optimizer='adam')


# Creamos el generador
# Parecido a autoencoders
# 784 (=28*28) --> 150 --> 30 --> 150 --> 784
coding_size = 100
# 100 --> 150 --> 784

generator = Sequential()
generator.add(Dense(100, input_shape=[coding_size], activation='relu'))
generator.add(Dense(150, activation='relu'))
generator.add(Dense(784, activation='tanh'))
generator.add(Reshape((28, 28)))
# Nota: No se compila el generador porque su entrenamiento depende de la respuesta
#   del discriminador dentro del modelo GAN combinado, no de un entrenamiento directo 
#   e independiente.

# Creamos el modelo combinado
gan = Sequential([generator, discriminator])
# Al compilar y entrenar el modelo combinado (gan), solo queremos que se actualicen los pesos del generador y no los del discriminador.
discriminator.trainable = False
gan.compile(loss='binary_crossentropy', optimizer='adam')
```

### Entrenar el modelo

```python
batch_size = 32
my_data = only_zeros
dataset = tf.data.Dataset.from_tensor_slices(my_data)
dataset = dataset.shuffle(buffer_size=1000)
dataset = dataset.batch(batch_size, drop_remainder=True).prefetch(1)
epochs = 1

generator, discriminator = gan.layers

for epoch in range(epochs):

    print(f"Currently on epoch {epoch}")

    for X_batch in dataset:
       i = i+1
       if i%100==0:
           print(i)
       print(f"Currently in batch number {i}")

       # Discriminator training
       noise = np.random.normal(size=(batch_size, coding_size))
       gen_images = generator(noise)
       # Concatenar imágenes reales y generadas
       X_fake_vs_real = tf.concat([gen_images, tf.dtypes.cast(X_batch, tf.float32)], axis=0)
       y1 = tf.constant(np.concatenate([[0.0]*batch_size] + [[1.0]*batch_size]))

       discriminator.trainable = True
       discriminator.train_on_batch(X_fake_vs_real, y1)

       # Generator training
       noise = np.random.normal(size=(batch_size, coding_size))
       y2 = tf.constant(np.concatenate([[1.0]*batch_size]))
       discriminator.trainable = False
       gan.train_on_batch(noise, y2)
```

### Generar imágenes

```python
# Generamos un vector de ruido
noise = tf.random.normal(shape=[10, coding_size])
noise.shape # (10, 100)
plt.imshow(noise)
plt.show()

# pasamos el ruido por el generador
images = generator(noise)
plt.imshow(images[0], cmap='gray')
plt.show() # Debe aparecer una imagen parecida a cero
```


