# Algoritmos

En python hay muchos algoritmos que nos pueden ayudar a realizar operaciones de forma más sencilla.

## Algoritmo de Kadane

La idea del algoritmo de Kadane es recorrer la matriz de izquierda a derecha y, para cada elemento, encontrar la suma máxima entre todas las submatrices que terminan en ese elemento . El resultado será el máximo de todos estos valores.

```python
def maxSubarraySum(arr):
    
    res = arr[0]
    maxEnding = arr[0]

    for i in range(1, len(arr)):
        
        # Encuentra el mayor valor entre:
        #  - Sumar el nuevo valor al sub-array actual
        #  - Empezar un nuevo sub-array
        maxEnding = max(maxEnding + arr[i], arr[i])
        
        # Actualizar resultado si la suma del nuevo
        #  sub-array es el nuevo mayor valor.
        res = max(res, maxEnding)
    
    return res

arr = [2, 3, -8, 7, -1, 2, 3]
print(maxSubarraySum(arr))
# Salida: 11 --> Generado por sub-array {7, -1, 2, 3}
```
