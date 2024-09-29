# Lectura y escritura

Se pueden leer tablas desde muchas fuentes diferentes como archivos csv, págias web, etc.

## Leer archivo csv

```python
import numpy as np
import pandas as pd

df = pd.read_csv('example.csv')
'''
df
 a b c d
0 0 1 2 3
1 4 5 6 7
2 8 9 10 11
3 12 13 14 15
'''
```

## Escribir arhicvo csv

```python
df.to_csv('example.csv',index=False)
```

## Leer tabla de página web (HTML)

Para ser capaz de leer una tabla de una página web (archivo HTML) es importante comprobar que el firewall de tu máquina no bloquee la librería pandas para acceder a internet.

Puede ser necesario instalar librerias extras como `lxml`, `html5lib`, `beautifulsoup4`.

```python
tables = pd.read_html('http://www.fdic.gov/bank/individual/failed/banklist.html')

# Mostras primeros 5 elementos de la tabla
tables[0].head()
```
