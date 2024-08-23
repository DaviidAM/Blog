# DataFrames 

Los **DataFrames** es de las piezas más importante de pandas y están directamente inspirados en el lenguaje de programación R. Podemos pensar en un DataFrame como un conjunto de objetos Serie unidos para compartir el mismo índice. 

## Crear DataFrame

```py
import pandas as pd
import numpy as np
from numpy.random import randint

columns= ['W', 'X', 'Y', 'Z'] # four columns
index= ['A', 'B', 'C', 'D', 'E'] # five rows

np.random.seed(42)
data = randint(-100,100,(5,4))
data
'''
array([[  2,  79,  -8, -86],
       [  6, -29,  88, -80],
       [  2,  21, -26, -13],
       [ 16,  -1,   3,  51],
       [ 30,  49, -48, -99]])
'''

df = pd.DataFrame(data,index,columns)
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''
```

## Recolectar datos

### Coger columnas

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df['W']
'''
A     2
B     6
C     2
D    16
E    30
Name: W, dtype: int32
'''

type(df['W'])
# pandas.core.series.Series

df[['W','Z']]
'''
    W   Z
A	  2	-86
B	  6	-80
C	  2	-13
D	 16  51
E	 30 -99
'''

df['new'] = df['W'] + df['Y']
df
'''
    W	  X	  Y	  Z  new
A	  2	 79	 -8	-86   -6
B	  6	-29	 88	-80   94
C	  2	 21	-26	-13  -24
D	 16	 -1	  3	 51   19
E	 30	 49	-48	-99  -18
'''
```

### Borrar columna
```py
df
'''
    W	  X	  Y	  Z  new
A	  2	 79	 -8	-86   -6
B	  6	-29	 88	-80   94
C	  2	 21	-26	-13  -24
D	 16	 -1	  3	 51   19
E	 30	 49	-48	-99  -18
'''

# axis=1 porque estamos trabajando con columnas
df.drop('new',axis=1)
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

# Los cambios no se aplican si no se reasigna la variable
df
'''
    W	  X	  Y	  Z  new
A	  2	 79	 -8	-86   -6
B	  6	-29	 88	-80   94
C	  2	 21	-26	-13  -24
D	 16	 -1	  3	 51   19
E	 30	 49	-48	-99  -18
'''
new_df = df.drop('new',axis=1)
new_df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''
```

### Coger fila

#### Usando nombre
```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df.loc['A']
'''
W     2
X    79
Y    -8
Z   -86
Name: A, dtype: int32
'''

df.loc[['A','C']]
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
C	  2	 21	-26	-13
'''
```

#### Usando índice
```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df.iloc[0]
'''
W     2
X    79
Y    -8
Z   -86
Name: A, dtype: int32
'''

df.iloc[0:2]
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
'''
```

### Borrar fila

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

# axis=0 porque estamos trabajando con filas
df.drop('C',axis=0)
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

# No se guardan los cambios si no reasignamos el valor
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''
```

### Seleccionar subconjunto

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df.loc[['A','C'],['W','Y']]
'''
    W	  Y
A	  2	 -8
C	  2	-26
'''
```

## Condicionales

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df>0 #Valores del dataframe >0
'''
     W	  X	    Y	    Z
A	True	True	False	False
B	True	False	True	False
C	True	True	False	False
D	True	False	True	True
E	True	True	False	False
'''

df[df>0] #Muestra los valores del dataframe cuyo valor >0 (si no lo cumple muestra NaN)
'''
	  W	     X  	  Y	    Z
A	  2	  79.0	  NaN	  NaN
B	  6	   NaN   88.0	  NaN
C	  2	  21.0	  NaN	  NaN
D  16	   NaN	  3.0	 51.0
E  30	  49.0	  NaN	  NaN
'''

df['X']>0 #Valores de la columna X cuyo valor >0
'''
A     True
B    False
C     True
D    False
E     True
Name: X, dtype: bool
'''

df[df['X']>0] #Muestra valores del dataframe cuyo valor de la columna X >0
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
C	  2	 21	-26	-13
E	 30	 49	-48	-99
'''

df[df['X']>0]['Y'] #Igual que el caso anterior pero muestra sólo columna Y
'''
A    -8
C   -26
E   -48
Name: Y, dtype: int32
'''

df[df['X']>0][['Y','Z']] #Igual que el caso anterior pero muestra sólo columnas Y Z
'''
     Y	  Z
A	  -8	-86
C	 -26	-13
E	 -48	-99
'''

df[(df['W']>0) & (df['Y'] > 1)] #Muestra dataframe que cumple valor de W >0 y valor de Y >1
'''
    W	    X	  Y	    Z
B	  6	  -29	  88	-80
D	 16	  -1	  3	   51
'''
```

## Otras operaciones con índices

### Resetear índice

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df.reset_index()
'''
	index	  W	  X	  Y	  Z
0	    A	  2	 79	 -8	-86
1	    B	  6	-29	 88	-80
2	    C	  2	 21	-26	-13
3	    D	 16	 -1	  3	 51
4	    E	 30	 49	-48	-99
'''
```

### Reasignar índice

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

newind = 'CA NY WY OR OO'.split()
# ['CA', 'NY', 'WY', 'OR', 'OO']

df['States'] = newind
'''
    W	  X	  Y	  Z States
A	  2	 79	 -8	-86   CA
B	  6	-29	 88	-80   NY
C	  2	 21	-26	-13   WY
D	 16	 -1	  3	 51   OR
E	 30	 49	-48	-99   OO
'''

df = df.set_index('States')
'''
	      W	  X	  Y	  Z
States				
CA	    2	 79	 -8	-86
NY	    6	-29	 88	-80
WY	    2	 21	-26	-13
OR	   16	 -1	  3	 51
OO	   30	 49	-48	-99
'''

df.columns
# Index(['W', 'X', 'Y', 'Z'], dtype='object')
```

## Obtener resumen del dataframe

```py
df
'''
    W	  X	  Y	  Z
A	  2	 79	 -8	-86
B	  6	-29	 88	-80
C	  2	 21	-26	-13
D	 16	 -1	  3	 51
E	 30	 49	-48	-99
'''

df.describe()
'''
      W	        X	          Y	          Z
count	5.00000	  5.000000	  5.000000	  5.000000
mean	11.20000	23.800000	  1.800000	  -45.400000
std	  11.96662	42.109381	  51.915316	  63.366395
min	  2.00000	  -29.000000	-48.000000	-99.000000
25%	  2.00000	  -1.000000	  -26.000000	-86.000000
50%	  6.00000	  21.000000	  -8.000000	  -80.000000
75%	  16.00000	49.000000	  3.000000	  -13.000000
max	  30.00000	79.000000	  88.000000	  51.000000
'''

df.dtypes
'''
W    int32
X    int32
Y    int32
Z    int32
dtype: object
'''

df.info()
'''
<class 'pandas.core.frame.DataFrame'>
Index: 5 entries, CA to OO
Data columns (total 4 columns):
W    5 non-null int32
X    5 non-null int32
Y    5 non-null int32
Z    5 non-null int32
dtypes: int32(4)
memory usage: 120.0+ bytes
'''
```

