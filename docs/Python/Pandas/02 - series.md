# Series 

**Pandas Series** es un tipo de dato muy similar a los arrays de NumPy, de hecho, es un tipo de de variable creado a partir de añadir nuevas funcionaliades al tipo array de NumPy.

Lo que diferencia a un array de NumPy de una Serie es que una Serie puede tener etiquetas de eje, lo que significa que puede ser indexada por una etiqueta, en lugar de solo una ubicación numérica. Además, no necesita contener datos numéricos, puede contener cualquier objeto arbitrario de Python.

## Crear series

Una vez que tengas instalada la librería de Pandas la añadiremos al script con un import. Es bastante usual añadir la librería con el nombre "pd".

```py
labels = ['a','b','c']
my_list = [10,20,30]
arr = np.array([10,20,30])
d = {'a':10,'b':20,'c':30}

# USANDO LISTAS
pd.Series(data=my_list)
'''
0    10
1    20
2    30
dtype: int64
'''

pd.Series(data=my_list,index=labels)
# pd.Series(my_list,labels) # lo mismo
'''
a    10
b    20
c    30
dtype: int64
'''


# USANDO NUMPY ARRAYS
pd.Series(arr)
'''
0    10
1    20
2    30
dtype: int32
'''
pd.Series(arr,labels)
'''
a    10
b    20
c    30
dtype: int32
'''

# USANDO DICCIONARIOS
pd.Series(d)
'''
a    10
b    20
c    30
dtype: int64
'''


# TIPOS DE DATOS EN SERIES
pd.Series(data=labels)
'''
0    a
1    b
2    c
dtype: object
'''
pd.Series([sum,print,len])
'''
0      <built-in function sum>
1    <built-in function print>
2      <built-in function len>
dtype: object
'''
```

## Índices

La clave para usar una Serie es comprender su índice. Pandas utiliza estos nombres o números de índice para permitir búsquedas rápidas de información (funciona como una tabla hash o un diccionario).

```py
ventas = pd.Series(data=[250,450,200,150],index = ['USA', 'China','India', 'Brazil'])
                                 
ventas
'''
USA       250
China     450
India     200
Brazil    150
dtype: int64
'''

nuevas_ventas = pd.Series([260,500,210,100],index = ['USA', 'China','India', 'Japan'])  

nuevas_ventas
'''
USA      260
China    500
India    210
Japan    100
dtype: int64
'''

ventas['USA']
# 250

# KEY ERROR!
#ventas['Russia'] # Nombre no existe en la serie
#ventas['USA '] #Incorrecto espacio en el nombre

ventas + nuevas_ventas
'''
Brazil      NaN
China     950.0
India     410.0
Japan       NaN
USA       510.0
dtype: float64
'''
# Más adelante explicaremos como solucionar este problema si los índices no coinciden.
```