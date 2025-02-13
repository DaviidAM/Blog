# Artificial Neural Networks (ANNs)

## Modelo del perceptron (Perceptron model)

```mermaid
flowchart LR
subgraph inputs
x1[x1]
x2[x2]
end

fx(("f(X)"))

subgraph Output
y[y]
end

x1 -- w1+b--> fx
x2 -- w2+b --> fx
fx --> y
```

`w: weight`

`b: bias`

>$\hat{y} = \sum_{i=1}^{n} x_iw_i + b_i$

and if B = b1+b2+...+bn

>$\hat{y} = B + \sum_{i=1}^{n} x_iw_i$

## Redes neuronales (Neural networks)

Las redes neuronales se generan al unir varios modelos de perceptrón en lo que se llama modelo del perceptrón multicapa (`multi-layer perceptron model`).

Las salidas de una capa de perceptrones, se usa como entrada para la siguinte capa de perceptrones.

Los perceptrones también se llaman neuronas.

```mermaid
flowchart LR
subgraph layer 1 - Input layer
l1p1((L1P1))
l1p2((L1P2))
l1pn((L1Pn))
end

subgraph layer 2
l2p1((L2P1))
l2p2((L2P2))
l2pn((L2Pn))
end

subgraph layer n
lnp1((LnP1))
lnp2((LnP2))
lnpn((LnPn))
end

subgraph Output - Output layer
o[output]
end

l1p1 --> l2p1
l1p1 --> l2p2
l1p1 --> l2pn

l1p2 ==> l2p1
l1p2 ==> l2p2
l1p2 ==> l2pn

l1pn -.-> l2p1
l1pn -.-> l2p2
l1pn -.-> l2pn

l2p1 --> lnp1
l2p1 --> lnp2
l2p1 --> lnpn

l2p2 ==> lnp1
l2p2 ==> lnp2
l2p2 ==> lnpn

l2pn -.-> lnp1
l2pn -.-> lnp2
l2pn -.-> lnpn

lnp1 --> o
lnp2 ==> o
lnpn -.-> o

```

`L: layer`

`P: perceptron`

* La primera capa se llama capa de entrada (input layer).
* La última capa se llama capa de salida (output layer). Esta última capa puede tener una o más neuronas.
* Las capas entre la capa de entrada y salida se llaman capas escondidas (hidden layers).

Se considera que una red es una redes neuronal profunda (`deep neural networks`) cuando contiene 2 o mñas capas escondidas.

## Funciones de activación (activation functions)

Estas funciones se usan para limitar la salida de cada neurona.

> $Z = W*x + b$

| Activation function | Traducción al español | Descripción |
| --- | --- | --- |
| Step function | Función paso | Salida es 0 si Z<=0, o salida es 1 si Z>0 |
| Sigmoid function | Función sigmoide | Parecida a la step function (mismos límites) pero más suave. $f(z) = \dfrac{1}{(1+e^{-z})}$ |
| Hyperbolic Tangent - tanh(z) | Tangente hiperbólica | Como la función sigmoide, pero en lugar de entre 0 y 1 es entre -1 y 1 |
| Rectified Linear Unit - ReLU | Relu | Salida es 0 si Z<=0, o salida es Z si Z>0. $f(z) = \max(0,z)$ |

To see more activation functions, see link below.

<https://en.wikipedia.org/wiki/Activation_function>
