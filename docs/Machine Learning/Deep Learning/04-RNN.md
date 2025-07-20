# Redes neuronales recurrentes - Recurrent Neural Networks (RNNs)

Las **redes neuronales recurrentes** es un tipo de red muy útil para trabajar con secuencias de datos.

> Antes de seguir con este punto es importante tener algunos conocimientos de redes neuronales con keras (librería de python), puedes ver [Keras](../Python/Keras/01%20-%20intro.md) para tener más información.

## Teoría básica

La idea de las redes neuronales recurrentes es intentar predecir el siguiente valor de una secuencia de datos, por ejemplo, si tenemos una secuencia de datos de precios de acciones, podemos usar una red neuronal recurrente para predecir el precio de la acción en el siguiente momento.

La diferencia principal con este tipo de redes neuranales es que se usa la salida de una iteración como realimentación de la entrada para la siguiente iteración.

## Ejemplos de secuencias

### De secuencia a secuencia (many to many)

En este caso, la entrada y la salida son secuencias de datos.

Ejemplo:
* Dada una secuencia de 5 palabras de entrada, la red neuronal predice las siguientes 5 palabras.

### De secuencia a un valor (many to vector)

En este caso, la entrada es una secuencia de datos y la salida es un valor.

Ejemplo:
* Dada una secuencia de 5 palabras de entrada, la red neuronal predice la siguiente palabra.

### De un valor a secuencia (vector to many)

En este caso, la entrada es un valor y la salida es una secuencia de datos.

Ejemplo:
* Dada una salabra de entrada, la red neuronal predice las siguientes 5 palabras.

## Limitaciones

* La mayor limitación de este tipo de redes neuronales es que sólo puede "recordar" los valores de la iteración anterior. Por lo tanto, conforme avanza la secuencia, la red neuronal pierde la información de los valores iniciales. Esto se resuelve con la red neuronal Long Short Term Memory (LSTM) o Gated Recurrent Unit (GRU).

* Otra gran desventaja de este tipo de redes durante el entrenamiento es el  "desvanecimiento del gradiente" (vanishing gradient).

### Desvanecimiento del gradiente

El problema del "desvanecimiento del gradiente" (vanishing gradient) ocurre porque, durante el entrenamiento de redes neuronales profundas o recurrentes, los valores de los gradientes (que indican cuánto deben ajustarse los pesos) se hacen cada vez más pequeños a medida que retroceden por las capas de la red. Esto sucede especialmente cuando se usan ciertas funciones de activación (como la sigmoide o tanh) y muchas capas o pasos temporales.

Cuando el gradiente se vuelve muy pequeño, la red "deja de aprender" en las capas más profundas o en los pasos iniciales de la secuencia, porque los ajustes en los pesos son casi nulos. Por eso, la red tiene dificultades para recordar información de largo plazo.

### Long Short Term Memory (LSTM)

El Long Short Term Memory (LSTM) es una red neuronal recurrente que puede "recordar" información de largo plazo.

La LSTM consta de 4 bloques principales:

#### Bloque 1: forget gate

El forget gate decide qué información se debe eliminar de la celda interna.

$$f_t = \sigma(W_f \cdot [h_{t-1}, x_t] + b_f)$$

#### Bloque 2: input gate

El input gate decide qué información se debe agregar a la celda interna.

$$i_t = \sigma(W_i \cdot [h_{t-1}, x_t] + b_i)$$

#### Bloque 3: cell state

El cell state es la celda interna de la LSTM.

$$C_t = f_t * C_{t-1} + i_t * tanh(W_C \cdot [h_{t-1}, x_t] + b_C)$$

#### Bloque 4: output gate

El output gate decide qué información se debe mostrar.

$$o_t = \sigma(W_o \cdot [h_{t-1}, x_t] + b_o)$$

### Gated Recurrent Unit (GRU)

El Gated Recurrent Unit (GRU) es una red neuronal recurrente que puede "recordar" información de largo plazo.

La GRU consta de 3 bloques principales:

#### Bloque 1: reset gate

El reset gate decide qué información se debe olvidar de la celda interna.

$$r_t = \sigma(W_r \cdot [h_{t-1}, x_t] + b_r)$$

#### Bloque 2: update gate

El update gate decide qué información se debe agregar a la celda interna.

$$z_t = \sigma(W_z \cdot [h_{t-1}, x_t] + b_z)$$

#### Bloque 3: hidden state

El hidden state es la celda interna de la GRU.

$$h_t = (1 - z_t) * h_{t-1} + z_t * tanh(W_h \cdot [r_t * h_{t-1}, x_t] + b_h)$$
