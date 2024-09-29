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
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''
```

## Recolectar datos

### Coger columnas

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
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
A   2 -86
B   6 -80
C   2 -13
D  16  51
E  30 -99
'''

df['new'] = df['W'] + df['Y']
df
'''
    W   X   Y   Z  new
A   2  79  -8 -86   -6
B   6 -29  88 -80   94
C   2  21 -26 -13  -24
D  16  -1   3  51   19
E  30  49 -48 -99  -18
'''
```

### Borrar columna

```py
df
'''
    W   X   Y   Z  new
A   2  79  -8 -86   -6
B   6 -29  88 -80   94
C   2  21 -26 -13  -24
D  16  -1   3  51   19
E  30  49 -48 -99  -18
'''

# axis=1 porque estamos trabajando con columnas
df.drop('new',axis=1)
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

# Los cambios no se aplican si no se reasigna la variable
df
'''
    W   X   Y   Z  new
A   2  79  -8 -86   -6
B   6 -29  88 -80   94
C   2  21 -26 -13  -24
D  16  -1   3  51   19
E  30  49 -48 -99  -18
'''
new_df = df.drop('new',axis=1)
new_df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''
```

### Coger fila

#### Usando nombre

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
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
    W   X   Y   Z
A   2  79  -8 -86
C   2  21 -26 -13
'''
```

#### Usando índice

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
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
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
'''
```

### Borrar fila

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

# axis=0 porque estamos trabajando con filas
df.drop('C',axis=0)
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
D  16  -1   3  51
E  30  49 -48 -99
'''

# No se guardan los cambios si no reasignamos el valor
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''
```

### Seleccionar subconjunto

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

df.loc[['A','C'],['W','Y']]
'''
    W   Y
A   2  -8
C   2 -26
'''
```

## Condicionales

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

df>0 #Valores del dataframe >0
'''
     W   X     Y     Z
A True True False False
B True False True False
C True True False False
D True False True True
E True True False False
'''

df[df>0] #Muestra los valores del dataframe cuyo valor >0 (si no lo cumple muestra NaN)
'''
   W      X     Y     Z
A   2   79.0   NaN   NaN
B   6    NaN   88.0   NaN
C   2   21.0   NaN   NaN
D  16    NaN   3.0  51.0
E  30   49.0   NaN   NaN
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
    W   X   Y   Z
A   2  79  -8 -86
C   2  21 -26 -13
E  30  49 -48 -99
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
     Y   Z
A   -8 -86
C  -26 -13
E  -48 -99
'''

df[(df['W']>0) & (df['Y'] > 1)] #Muestra dataframe que cumple valor de W >0 y valor de Y >1
'''
    W     X   Y     Z
B   6   -29   88 -80
D  16   -1   3    51
'''
```

## Otras operaciones con índices

### Resetear índice

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

df.reset_index()
'''
 index   W   X   Y   Z
0     A   2  79  -8 -86
1     B   6 -29  88 -80
2     C   2  21 -26 -13
3     D  16  -1   3  51
4     E  30  49 -48 -99
'''
```

### Reasignar índice

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

newind = 'CA NY WY OR OO'.split()
# ['CA', 'NY', 'WY', 'OR', 'OO']

df['States'] = newind
'''
    W   X   Y   Z States
A   2  79  -8 -86   CA
B   6 -29  88 -80   NY
C   2  21 -26 -13   WY
D  16  -1   3  51   OR
E  30  49 -48 -99   OO
'''

df = df.set_index('States')
'''
       W   X   Y   Z
States    
CA     2  79  -8 -86
NY     6 -29  88 -80
WY     2  21 -26 -13
OR    16  -1   3  51
OO    30  49 -48 -99
'''

df.columns
# Index(['W', 'X', 'Y', 'Z'], dtype='object')
```

## Obtener resumen del dataframe

```py
df
'''
    W   X   Y   Z
A   2  79  -8 -86
B   6 -29  88 -80
C   2  21 -26 -13
D  16  -1   3  51
E  30  49 -48 -99
'''

df.describe()
'''
      W         X           Y           Z
count 5.00000   5.000000   5.000000   5.000000
mean 11.20000 23.800000   1.800000   -45.400000
std   11.96662 42.109381   51.915316   63.366395
min   2.00000   -29.000000 -48.000000 -99.000000
25%   2.00000   -1.000000   -26.000000 -86.000000
50%   6.00000   21.000000   -8.000000   -80.000000
75%   16.00000 49.000000   3.000000   -13.000000
max   30.00000 79.000000   88.000000   51.000000
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

## Celdas sin definir

Podemos encontrarnos en un escenario donde un dataframe no tenga algunas celdas definidas, para estos casos tenemos 3 opciones para trabajar con dichas celdas:

1. Dejar la celda sin definir.
2. Eliminar las celdas sin definir.
3. Sobreescribir las celdas sin definir.

```py

df = pd.DataFrame({'A':[1,2,np.nan,4],
                  'B':[5,np.nan,np.nan,8],
                  'C':[10,20,30,40]})

# 1. Dejar celdas sin definir
df
'''
 A     B     C
0 1.0     5.0     10
1 2.0     NaN     20
2 NaN     NaN     30
3 4.0     8.0     40
'''

# 2. Eliminar las celdas sin definir.
df.dropna() # Elimina todas las filas/columnas que tengan al menos un valor sin definir.
'''
 A     B     C
0 1.0     5.0     10
3 4.0     8.0     40
'''

df.dropna(axis=1) # Elimina todas las columnas que tengan al menos un valor sin definir.
'''
    C
0 10
1 20
2 30
3 40
'''

df.dropna(thresh=2) # Elimina todas las filas/columnas que tengan al menos 2 valor sin definir.
'''
 A     B     C
0 1.0     5.0     10
1 2.0     NaN     20
3 4.0     8.0     40
'''

# 3. Sobreescribir las celdas sin definir.
df.fillna(value='FILL VALUE') # Este comando no sobrescribe el dataframe original. 
'''
    A         B         C
0 1         5         10
1 2         FILL VALUE 20
2 FILL VALUE FILL VALUE 30
3 4         8         40
'''

df['A'].fillna(value=0) # Modificar sólo columna A.
'''
0    1.0
1    2.0
2    0.0
3    4.0
Name: A, dtype: float64
'''

df.fillna(df.mean()) # Modificar celdas sin definir por la media de cada columna.
'''
 A         B     C
0 1.000000 5.0     10
1 2.000000 6.5     20
2 2.333333 6.5     30
3 4.000000 8.0     40
'''
```

## Agrupar valores

Podemos encontrarnos con tablas que tengan valores en común, por ejemplo una tabla con información de alumnos, con una columna que especifique el año de nacimiento, y queremos agrupar los alumnos por el año de nacimiento. Usando agrupaciones podemos sacar datos de grupos de alumnos que tengan el mismo año de nacimiento.

```py
df
'''
    Col1    Col2
0   A       4
1   B       3
2   A       6
3   C       2       
4   C       1
'''

df.groupby('Col1').sum() # Sumar valores agrupados por Col1
'''
     Col2
Col1 
A     10
B     3
C     3
'''
```

También se pueden crear grupos usando más de una key.

```py
df.groupby(['Year','Country']).mean()
```

### Funciones de agregación

<table><td><tt
><span
>count</span></tt></td><td>Number of non-null observations</td></tr><tr
><td><tt
><span
>sum</span></tt></td><td>Sum of values</td></tr><tr
><td><tt
><span
>mean</span></tt></td><td>Mean of values</td></tr><tr
><td><tt
><span
>mad</span></tt></td><td>Mean absolute deviation</td></tr><tr
><td><tt
><span
>median</span></tt></td><td>Arithmetic median of values</td></tr><tr
><td><tt
><span
>min</span></tt></td><td>Minimum</td></tr><tr
><td><tt
><span
>max</span></tt></td><td>Maximum</td></tr><tr
><td><tt
><span
>mode</span></tt></td><td>Mode</td></tr><tr
><td><tt
><span
>abs</span></tt></td><td>Absolute Value</td></tr><tr
><td><tt
><span
>prod</span></tt></td><td>Product of values</td></tr><tr
><td><tt
><span
>std</span></tt></td><td>Unbiased standard deviation</td></tr><tr
><td><tt
><span
>var</span></tt></td><td>Unbiased variance</td></tr><tr
><td><tt
><span
>sem</span></tt></td><td>Unbiased standard error of the mean</td></tr><tr
><td><tt
><span
>skew</span></tt></td><td>Unbiased skewness (3rd moment)</td></tr><tr
><td><tt
><span
>kurt</span></tt></td><td>Unbiased kurtosis (4th moment)</td></tr><tr
><td><tt
><span
>quantile</span></tt></td><td>Sample quantile (value at %)</td></tr><tr
><td><tt
><span
>cumsum</span></tt></td><td>Cumulative sum</td></tr><tr
><td><tt
><span
>cumprod</span></tt></td><td>Cumulative product</td></tr><tr
><td><tt
><span
>cummax</span></tt></td><td>Cumulative maximum</td></tr><tr
><td><tt
><span
>cummin</span></tt></td><td>Cumulative minimum</td></tr></tbody></table>

## Operaciones

```py
df_one = pd.DataFrame({'k1':['A','A','B','B','C','C'],
                      'col1':[100,200,300,300,400,500],
                      'col2':['NY','CA','WA','WA','AK','NV']})

'''
 k1 col1 col2
0 A 100     NY
1 A 200     CA
2 B 300     WA
3 B 300     WA
4 C 400     AK
5 C 500     NV
'''
```

### Valores únicos

```py
# Devuelve valores únicos de la columna col2
df_one['col2'].unique() 
# array(['NY', 'CA', 'WA', 'AK', 'NV'], dtype=object)

# Devuelve número valores únicos de la columna col2
df_one['col2'].nunique()
# 5

# Devuelve tabla con cuenta de cada valor único
df_one['col2'].value_counts()
'''
WA    2
CA    1
NV    1
NY    1
AK    1
Name: col2, dtype: int64
'''

# Eliminar filas duplicadas
df_one.drop_duplicates()
'''
 k1 col1 col2
0 A 100     NY
1 A 200     CA
2 B 300     WA
4 C 400     AK
5 C 500     NV
'''
```

### Crear nueva columna con operaciones y funciones

```py
df_one
'''
 k1 col1 col2
0 A 100     NY
1 A 200     CA
2 B 300     WA
3 B 300     WA
4 C 400     AK
5 C 500     NV
'''

# nueva columna 'New col' dependiente de 'col1'
df_one['New Col'] = df_one['col1'] * 10

df_one
'''
 k1 col1 col2 New Col
0 A 100     NY     1000
1 A 200     CA     2000
2 B 300     WA     3000
3 B 300     WA     3000
4 C 400     AK     4000
5 C 500     NV     5000
'''

# Aplicar funciones a una columna
def grab_first_letter(state):
    # Devuelve primera letra del valor
    return state[0]

df_one['col2'].apply(grab_first_letter)
'''
0    N
1    C
2    W
3    W
4    A
5    N
Name: col2, dtype: object
'''

df_one['first letter'] = df_one['col2'].apply(grab_first_letter)
'''
 k1 col1 col2 New Col     first letter
0 A 100     NY     1000     N
1 A 200     CA     2000     C
2 B 300     WA     3000     W
3 B 300     WA     3000     W
4 C 400     AK     4000     A
5 C 500     NV     5000     N
'''
```

### Mapas

```py
df_one['k1']
'''
0    A
1    A
2    B
3    B
4    C
5    C
Name: k1, dtype: object
'''

df_one['k1'].map({'A':1,'B':2,'C':3})
'''
0    1
1    1
2    2
3    2
4    3
5    3
Name: k1, dtype: int64
'''
```

### Localizar maximos y mínimos

```py
# Valor máximo de una columna
df_one['col1'].max()
# 500

# Valor mínimo de una columna
df_one['col1'].min()
# 100

# Índice con valor máximo de una columna
df_one['col1'].idxmax()
# 500

# VÍndice con valor mínimo de una columna
df_one['col1'].idxmin()
# 100
```

### Conseguir nombres de columnas e índices

```py
# Conseguir nombres de columnas
df_one.columns
# Index(['k1', 'col1', 'col2', 'New Col', ... ], dtype='object')

# Índices
df_one.index
# RangeIndex(start=0, stop=6, step=1)

# Cambiar nombres de columnas
df_one.columns = ['C1','C2','C3', 'C4']
'''
 C1 C2     C3     C4 
0 A 100     NY     1000
1 A 200     CA     2000
2 B 300     WA     3000
3 B 300     WA     3000
4 C 400     AK     4000
5 C 500     NV     500
'''
```

### Ordenar tabla con valores de una columna

```py
df_one
'''
    C1  C2     C3     C4 
0   A   100     NY     1000
1   A   200     CA     2000
2   B   300     WA     3000
3   B   300     WA     3000
4   C   400     AK     4000
5   C   500     NV     500
'''

df_one.sort_values('C3')
'''
 C1 C2     C3     C4 
4 C 400     AK     4000
1 A 200     CA     2000
5 C 500     NV     5000
0 A 100     NY     1000
2 B 300     WA     3000
3 B 300     WA     3000
'''
```

### Concatenar tablas

```py
features = pd.DataFrame({'A':[100,200,300,400,500],
                        'B':[12,13,14,15,16]})
predictions = pd.DataFrame({'pred':[0,1,1,0,1]})

features
'''
    A     B
0 100     12
1 200     13
2 300     14
3 400     15
4 500     16
'''

predictions
'''
 pred
0 0
1 1
2 1
3 0
4 1
'''

# Concatenar = Unir tablas
pd.concat([features,predictions]) # Atención con los ejes
'''
 A     B     pred
0 100.0 12.0 NaN
1 200.0 13.0 NaN
2 300.0 14.0 NaN
3 400.0 15.0 NaN
4 500.0 16.0 NaN
0 NaN     NaN     0.0
1 NaN     NaN     1.0
2 NaN     NaN     1.0
3 NaN     NaN     0.0
4 NaN     NaN     1.0
'''

pd.concat([features,predictions],axis=1) # Atención con los ejes
'''
 A     B     pred
0 100     12     0
1 200     13     1
2 300     14     1
3 400     15     0
4 500     16     1
'''
```

### Crear variable dummy

```py
df_one['C1']
'''
0    A
1    A
2    B
3    B
4    C
5    C
Name: C1, dtype: object
'''

pd.get_dummies(df_one['C1'])
'''
 A B C
0 1 0 0
1 1 0 0
2 0 1 0
3 0 1 0
4 0 0 1
5 0 0 1
'''
```
