# Excepciones

Las excepciones en Python son una forma de manejar errores que pueden ocurrir durante la ejecución de un programa. Cuando ocurre un error, Python genera una excepción que puede ser capturada y manejada para evitar que el programa se detenga abruptamente.

Ejemplo básico:

```python
try:
    # Código que puede causar una excepción
    resultado = 10 / 0
except ZeroDivisionError:
    # Código que se ejecuta si ocurre una excepción de tipo ZeroDivisionError
    print("No se puede dividir por cero.")
else:
    # Código que se ejecuta si no ocurre ninguna excepción
    print("El resultado es:", resultado)
finally:
    # Código que se ejecuta siempre, ocurra o no una excepción
    print("Fin del bloque try-except.")
```

En este ejemplo:

1. **try**: Contiene el código que puede causar una excepción.
2. **except**: Captura y maneja la excepción específica (en este caso, ZeroDivisionError).
3. **else**: Se ejecuta si no ocurre ninguna excepción.
4. **finally**: Se ejecuta siempre, independientemente de si ocurrió una excepción o no.

## Manejo de múltiples excepciones

```python
try:
    # Intentar acceder a un índice fuera del rango
    lista = [1, 2, 3]
    elemento = lista[5]
except IndexError as e:
    print("Error: Índice fuera del rango.")  
    # Salida: Error: Índice fuera del rango.
    print("Detalles del error:", e)  
    # Salida: list index out of range
except Exception as e:
    print("Error genérico:", e)
```

## Uso de else y finally en el manejo de excepciones

```python
try:
    # Intentar convertir una cadena a entero
    numero = int("123")
except ValueError as e:
    print("Error: No se puede convertir la cadena a entero.")
else:
    print("Conversión exitosa:", numero)  
    # Salida: Conversión exitosa: 123
finally:
    print("Bloque finally ejecutado.")  
    # Salida: Bloque finally ejecutado.
```

## Manejar varios tipos de errores en la misma excepción

```python
try:
    # Código que puede causar múltiples excepciones
    numero = int(input("Introduce un número: "))
    resultado = 10 / numero
except (ValueError, ZeroDivisionError) as e:
    # Maneja tanto ValueError como ZeroDivisionError
    print(f"Ocurrió un error: {e}")
else:
    print(f"El resultado es: {resultado}")
finally:
    print("Fin del bloque try-except.")
```

## Lanzar una excepción personalizada

```python
try:
    # Definir una función que lanza una excepción personalizada
    def verificar_edad(edad):
        if edad < 18:
            raise ValueError("La edad debe ser mayor o igual a 18.")
        return True

    # Llamar a la función con una edad inválida
    verificar_edad(16)
except ValueError as e:
    print("Error personalizado:", e)  
    # Salida: Error personalizado: La edad debe ser mayor o igual a 18.
```

## Manejo de excepciones anidadas

```python
try:
    try:
        # Intentar abrir un archivo que no existe
        with open("archivo_inexistente.txt", "r") as archivo:
            contenido = archivo.read()
    except FileNotFoundError as e:
        print("Error: Archivo no encontrado.")  
        # Salida: Error: Archivo no encontrado.
        print("Detalles del error:", e)  
        # Salida: [Errno 2] No such file or directory: 'archivo_inexistente.txt'
        raise  # Re-lanzar la excepción
except Exception as e:
    print("Excepción capturada en el bloque externo:", e)
```

## Uso de assert para lanzar una excepción

```python
try:
    # Verificar una condición con assert
    x = 10
    assert x > 20, "x debe ser mayor que 20"
except AssertionError as e:
    print("Error de aserción:", e)  
    # Salida: Error de aserción: x debe ser mayor que 20
```
