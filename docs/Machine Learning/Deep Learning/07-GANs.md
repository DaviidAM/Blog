# Generative Adversarial Networks (GANs)

Las GANs (Generative Adversarial Networks, o Redes Generativas Antagónicas) son un tipo de arquitectura de redes neuronales profundas que se utilizan principalmente para generar datos nuevos que son similares a un conjunto de datos real.

Consisten en dos redes neuronales que compiten entre sí:

1. **Generador:** Esta red intenta crear datos falsos (por ejemplo, imágenes) que se parezcan lo máximo posible a los datos reales.
2. **Discriminador:** Esta red intenta distinguir entre los datos reales y los datos generados por el generador.

Ambas redes se entrenan juntas en un proceso de competencia (de ahí el término “adversarial”):

- El generador aprende a crear datos cada vez más realistas para “engañar” al discriminador.
- El discriminador aprende a ser cada vez mejor detectando si los datos son reales o generados.

Este “juego” continúa hasta que el generador produce datos tan realistas que el discriminador no puede diferenciarlos de los reales con precisión.


### Python example

[GANs Python example](../../Python/Keras/07%20-%20GANs.md)

