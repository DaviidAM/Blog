# Funciones

Las funciones te permiten agrupar código que realiza una tarea específica y reutilizarlo.

Estas funciones pueden recibir variables de entrada (parámetros) desde el código que llama a dicha función para así utilizar dicho valor en la lógica de la función.

Ejemplo:

```python
def saludar(nombre):
    """Esta función saluda a la persona cuyo nombre se pasa como argumento."""
    print(f"Hola, {nombre}!")

saludar("Juan")  # Imprime: Hola, Juan!
```

También se pueden definir funciones con valor de retorno:

```python
def suma(a, b):
    return a + b

resultado = suma(3, 5)
print(resultado)  # Imprime: 8
```
