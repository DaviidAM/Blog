# NLP en Keras

Como ya explicamos en la teoría de **redes para procesamiento de lenguaje natural**, estas redes neuronales son muy útil cuando necesitamos generar texto.

[Teoría NLPs](../../Machine%20Learning/Deep%20Learning/05-NLP.md)

## Ejemplo de NLP con Keras

### Preparación de datos

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

import tensorflow as tf

#Vamos a usar texto de Shakespeare para entrenar el modelo ya que posee caracteristicas especiales en su forma de escribir.

path_to_file = "shakespeare.txt"

text = open(path_to_file, 'r').read()

# Calculamos los caracteres unicos en el texto.
vocab = sorted(set(text))
print(f"{len(vocab)} caracteres unicos en el texto.") # 84 caracteres unicos en el texto.

# Transformamos cada caracter a un índice/número
char_to_ind = {char:ind for ind,char in enumerate(vocab)}
# Ejemplo: char_to_ind['H'] = 33

# Ahora realizamos la operacion inversa
ind_to_char = np.array(vocab)
# Ejemplo: ind_to_char[33] = 'H'

# Pasamos el texto original a un array de números
encoded_text = np.array([char_to_ind[c] for c in text])
print(encoded_text.shape) # (5445609,)
```

### Crear batches

```python
# La forma de escribir de Shakespeare es con frases cortas, y relacionando frases con frases parecido a poesía.
# Por lo que vamos a crear batches que contengan al menos 3 frases completas.
# Si una frase tiene 40 caracteres aproximadamente, entonces vamos a coger 120 caracteres para cada batch.
seq_length = 120

# calculamos el número total de secuencias
total_num_seq = len(text) // (seq_length+1)  # 45005

# Creamos un dataset de caracteres
char_dataset = tf.data.Dataset.from_tensor_slices(encoded_text)
type(char_dataset) # <class 'tensorflow.python.data.ops.dataset_ops.TensorSliceDataset'>

'''
# Mostramos los primeros 500 caracteres del texto en forma de dataset (entradas de la red).
for item in char_dataset.take(500):
    print(ind_to_char[item.numpy()])
'''

# Creamos batches de secuencias (entradas de la red - igual que en el ejemplo anterior pero lo hace el método automaticamente).
sequence_dataset = char_dataset.batch(seq_length+1, drop_remainder=True)

def create_seq_target(seq):
    input_txt = seq[:-1] # Hello worl
    target_txt = seq[1:] # ello world
    return input_txt, target_txt

# Creamos el dataset de secuencias
dataset = sequence_dataset.map(create_seq_target)

# Ejemplo
for input_txt, target_txt in dataset.take(1):
    print(f"Input: {''.join(ind_to_char[input_txt.numpy()])}")
    print(f"Target: {''.join(ind_to_char[target_txt.numpy()])}")

# Parametros para crear el dataset
batch_size = 128
buffer_size = 10000

# Mezclamos el dataset y lo dividimos en batches
dataset = dataset.shuffle(buffer_size).batch(batch_size, drop_remainder=True)
```

### Crear el modelo

```python
vocab_size = len(vocab) # 84 = número de caracteres únicos en el texto.
embedding_dim = 64 # Con este valor se puede jugar un poco, pero debe ser cercano a la cantidad de caracteres (vocab_size).

rnn_neurons = 1026 # Con este valor se puede jugar un poco.

from tensorflow.keras.losses import sparse_categorical_crossentropy

# Creamos la función de pérdida personalizada porque el from_logits=True es necesario
#  para que la función de pérdida funcione correctamente en este caso.
def custom_sparse_cat_loss(y_true, y_pred): 
    return sparse_categorical_crossentropy(y_true, y_pred, from_logits=True)    

from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Embedding, GRU, Dense

def create_model(vocab_size, embedding_dim, rnn_neurons, batch_size):
    model = Sequential()
    model.add(Embedding(input_dim=vocab_size, output_dim=embedding_dim, batch_input_shape=[batch_size, None]))
    model.add(GRU(rnn_neurons, return_sequences=True, stateful=True, recurrent_initializer='glorot_uniform'))
    model.add(Dense(vocab_size))

    model.compile(optimizer='adam', loss=custom_sparse_cat_loss)

    return model

model = create_model(vocab_size, embedding_dim, rnn_neurons, batch_size)
# model.summary()
```

### Entrenar el modelo

```python
# Para entrenar el modelo es exactamente igual que como vimos con RNN.
# Por lo que en este caso vamos a cargar los pesos de un modelo ya entrenado.

from tensorflow.keras.models import load_model

batch_size = 1
model = create_model(vocab_size, embed_dim, rnn_neurons, batch_size=batch_size)

model.load_weights('<file_name>.h5')

model.build(tf.TensorShape([batch_size, None]))
```

### Generar texto

```python
# Creamos una función para generar texto.
def generate_text(model, start_seed, gen_size=500, temp = 1.0):
    
    num_generate = gen_size
    
    # Convertimos el string de inicio a números.
    input_eval = [char_to_ind[s] for s in start_seed]
    
    # Expandimos la dimensión del input_eval para que sea compatible con el modelo.
    input_eval = tf.expand_dims(input_eval, 0)

    text_generated = []

    # Este es un parámetro que controla la aleatoriedad de las predicciones y podemos modificarlo para obtener diferentes resultados.
    temperature = temp

    model.reset_states()
    for i in range(num_generate):
        # predecimos la siguiente letra
        predictions = model(input_eval)
        
        # Quitamos la dimensión del batch
        predictions = tf.squeeze(predictions, 0)

        # Aplicamos la temperatura
        predictions = predictions / temperature
        
        # Categorizamos las predicciones
        predicted_id = tf.random.categorical(predictions, num_samples=1)[-1,0].numpy()
        
        # Actualizamos el input_eval
        input_eval = tf.expand_dims([predicted_id], 0)
        
        # Agregamos la letra predicha al texto generado
        text_generated.append(ind_to_char[predicted_id])
    return (start_seed + ''.join(text_generated))


# Generamos texto
print(generate_text(model, start_seed="ROMEO: ", gen_size=1000, temp=1.0))
```
