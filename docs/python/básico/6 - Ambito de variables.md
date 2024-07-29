# Ámbito de Variables

Las variables definidas dentro de una función tienen un ámbito local, mientras que las variables definidas fuera de las funciones tienen un ámbito global.

Ejemplo:

```python
x = 10  # Variable global

def funcion():
    y = 5  # Variable local
    print(x)  # Accede a la variable global
    print(y)  # Accede a la variable local

funcion()
print(x)  # Imprime: 10
# print(y)  # Esto causará un error porque y no está definida en el ámbito global
```
