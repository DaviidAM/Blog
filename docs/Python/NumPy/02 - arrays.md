# Arrays

En Python, un array en la librería NumPy es una estructura de datos fundamental para trabajar con conjuntos de números. Imagina una lista muy organizada y eficiente, diseñada específicamente para realizar cálculos numéricos de manera rápida.

Se pueden crear NumPy arrays directamente desde una lista de Python.

```py
my_list = [1,2,3,4]
np.array(my_list) # array([1, 2, 3])
```

NumPy array también puede trabajar con más de una dimensión.

```py
my_matrix = [[1,2,3],[4,5,6],[7,8,9]]
np.array(my_matrix) 
'''
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
'''
```

## Arrange

Sirve para generar un array con valores igualmente espaciados dentro de un rango definido.

```py
np.arange(15)
# array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
np.arange(5,10)
#array([5, 6, 7, 8, 9])
np.arange(0,12,3)
# array([0, 3, 6, 9])
```

## Zeros y Unos

Para tema de machine learning es muy importante poder generar un array o matriz de ceros o unos, por lo que NumPy ofrece directamente un método para ello.

```py
np.zeros(3) 
# array([0., 0., 0.])
np.zeros((2,5,5)) # 2 matrices de 5x5 
'''
array([[[0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.]],

       [[0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0.]]])
'''
np.ones(3)
# array([1., 1., 1.])
np.ones((3,3))
'''
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
'''
```

## Linspace

Devuelve números espaciados uniformemente en un intervalo especificado.

```py
np.linspace(0,10,3) 
# array([ 0.,  5., 10.])
np.linspace(0,10,20) # 20 valores x-espaciados entre 0 y 10
'''
array([ 0.        ,  0.52631579,  1.05263158,  1.57894737,  2.10526316,
        2.63157895,  3.15789474,  3.68421053,  4.21052632,  4.73684211,
        5.26315789,  5.78947368,  6.31578947,  6.84210526,  7.36842105,
        7.89473684,  8.42105263,  8.94736842,  9.47368421, 10.        ])
'''
np.linspace(0,10,21) # 21 valores x-espaciados entre 0 y 10
'''
array([ 0. ,  0.5,  1. ,  1.5,  2. ,  2.5,  3. ,  3.5,  4. ,  4.5,  5. ,
        5.5,  6. ,  6.5,  7. ,  7.5,  8. ,  8.5,  9. ,  9.5, 10. ])
'''
```

## Eye

Devuelve la matriz identidad de la dimensión que se le indique.

```py
np.eye(6)
'''
array([[1., 0., 0., 0., 0., 0.],
       [0., 1., 0., 0., 0., 0.],
       [0., 0., 1., 0., 0., 0.],
       [0., 0., 0., 1., 0., 0.],
       [0., 0., 0., 0., 1., 0.],
       [0., 0., 0., 0., 0., 1.]])
'''
```

## Random

Paquete dentro de la librería NumPy que se encarga de generar arrays aleatorios.

### rand

Genera números aleatorios siguiendo una distribución uniforme. Los valores generados se encuentran en el intervalo `[0, 1)`, es decir, desde 0 (inclusive) hasta 1 (exclusivo).

```py
np.random.rand(3)
# array([0.36113539, 0.88446185, 0.48074398])
np.random.rand(2,3,5) # 2 matrices de 3x5

'''
array([[[0.04633713, 0.53137266, 0.74117606, 0.20512219, 0.6168586 ],
        [0.06443939, 0.34243107, 0.3059021 , 0.20598129, 0.47033699],
        [0.9692372 , 0.41828453, 0.74705593, 0.57757702, 0.30476353]],

       [[0.49879649, 0.42105791, 0.51103856, 0.45574341, 0.798533  ],
        [0.79231305, 0.08936987, 0.68506619, 0.93837446, 0.02559339],
        [0.83318277, 0.40604363, 0.69039322, 0.68334075, 0.93850892]]])
'''
```

### randn

Genera números aleatorios siguiendo una distribución normal estándar. Los valores generados pueden ser cualquier número real, pero la mayoría se concentrará alrededor de 0.

```py
np.random.randn(2)
# array([ 0.18584968, -0.1794187 ])
np.random.randn(5,5)

'''
array([[-0.20626407,  1.16792749, -0.4729137 ,  1.13940936, -0.59339953],
       [-0.99356095, -0.14937524,  1.55929908, -3.18371919,  0.66314423],
       [ 0.73271548, -0.59890564,  0.36802319,  0.63084196,  0.59189165],
       [-2.00064426,  0.14159769, -1.36269102,  1.52719339, -0.70780552],
       [ 0.18033592, -0.12497444, -0.40132878,  1.67288472,  0.45831362]])
'''
```

### randint

Genera números aleatorios enteros entre el rango de valores definido.

```py
np.random.randint(1,100) # Valor entre 1 y 100
# 88
np.random.randint(1,100,10) # 10 valores entre 1 y 100
'''
array([39, 50, 72, 18, 27, 59, 15, 97, 11, 14])
'''
```

### seed

Sirve para cambiar la semilla utilizada para generar números aleatorios, usando la misma semilla se generarán los mismos números aleatorios.

```py
np.random.seed(42)
np.random.rand(4)
# array([0.37454012, 0.95071431, 0.73199394, 0.59865848])
np.random.seed(42)
np.random.rand(4)
# array([0.37454012, 0.95071431, 0.73199394, 0.59865848])
```

## Métodos y atributos de los arrays

### Reshape

Cambia la dimensión de un array.

```py
arr = np.arange(25)
# arr = array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24])
arr.reshape(5,5)
'''
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14],
       [15, 16, 17, 18, 19],
       [20, 21, 22, 23, 24]])
'''
```

### max, min, argmax, argmin

Estos son métodos útiles para encontrar valores máximos o mínimos. O para encontrar las ubicaciones de sus índices usando argmin o argmax.

```py
ranarr = np.random.rand(4)
# ranarr = array([0.37454012, 0.95071431, 0.73199394, 0.59865848])
ranarr.max() # 0.9507143064099162 - Valor max
ranarr.argmax() # 1 - Posición del primer valor max
ranarr.min() # 0.3745401188473625 - Valor min
ranarr.argmin() # 0 - Posición del valor min
ranarr.dtype # dtype('float64') - Tipo de datos dentro del array
```

### shape

Devuelve la dimensión del array
```py
ranarr = np.random.rand(4)
# ranarr = array([0.37454012, 0.95071431, 0.73199394, 0.59865848])
ranarr.shape
# (4,)
new= ranarr.reshape(2,2)
new.shape
# (2, 2)
```

