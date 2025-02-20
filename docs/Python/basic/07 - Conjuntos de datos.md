# Conjuntos de datos

Cada uno de los conjuntos de datos tiene sus propias ventajas y desventajas. Las listas son ideales para colecciones ordenadas y mutables, las tuplas para datos inmutables, los sets para colecciones de elementos únicos y los diccionarios para pares clave-valor. Conocer cuándo y cómo usar cada uno te ayudará a escribir código más eficiente y claro en Python.

## Lista

Las listas son colecciones ordenadas y mutables, lo que significa que puedes cambiar sus elementos después de haberlas creado. Se definen utilizando corchetes [].

```python
# Crear una lista inicial
frutas = ["manzana", "banana", "cereza"]
print("Lista inicial:", frutas)  
# Salida: ['manzana', 'banana', 'cereza']

# Leer un elemento de la lista
print("Primer elemento de la lista:", frutas[0])  
# Salida: 'manzana'

# Añadir un elemento al final de la lista
frutas.append("naranja")
print("Después de append:", frutas)  
# Salida: ['manzana', 'banana', 'cereza', 'naranja']

# Añadir todos los elementos de otra lista
frutas.extend(["kiwi", "mango"])
print("Después de extend:", frutas)  
# Salida: ['manzana', 'banana', 'cereza', 'naranja', 'kiwi', 'mango']

# Insertar un elemento en una posición específica
frutas.insert(1, "fresa")
print("Después de insert:", frutas)  
# Salida: ['manzana', 'fresa', 'banana', 'cereza', 'naranja', 'kiwi', 'mango']

# Eliminar el primer elemento con el valor especificado
frutas.remove("banana")
print("Después de remove:", frutas)  
# Salida: ['manzana', 'fresa', 'cereza', 'naranja', 'kiwi', 'mango']

# Eliminar el elemento en la posición especificada y devolverlo
fruta_eliminada = frutas.pop(2)
print("Elemento eliminado con pop:", fruta_eliminada)  
# Salida: cereza
print("Después de pop:", frutas)  
# Salida: ['manzana', 'fresa', 'naranja', 'kiwi', 'mango']

# Eliminar todos los elementos de la lista
frutas.clear()
print("Después de clear:", frutas)  
# Salida: []

# Crear una nueva lista para los siguientes ejemplos
frutas = ["manzana", "banana", "cereza", "banana"]

# Obtener tamaño de la lista
print("Tamaño de la lista:", len(frutas))  
# Salida: 4

# Obtener el índice del primer elemento con el valor especificado
indice = frutas.index("banana")
print("Índice de 'banana':", indice)  
# Salida: 1

# Contar el número de elementos con el valor especificado
conteo = frutas.count("banana")
print("Conteo de 'banana':", conteo)  
# Salida: 2

# Ordenar los elementos de la lista en orden ascendente
numeros = [3, 1, 4, 1, 5, 9]
numeros.sort()
print("Después de sort:", numeros)  
# Salida: [1, 1, 3, 4, 5, 9]

# Invertir el orden de los elementos en la lista
frutas.reverse()
print("Después de reverse:", frutas)  
# Salida: ['banana', 'cereza', 'banana', 'manzana']

# Devolver una copia superficial de la lista
copia_frutas = frutas.copy()
print("Copia de la lista:", copia_frutas)  
# Salida: ['banana', 'cereza', 'banana', 'manzana']
```

## Tupla

Las tuplas son similares a las listas, pero son inmutables, lo que significa que no puedes cambiar sus elementos después de haberlas creado. Se definen utilizando paréntesis ().

```python
# Ejemplo de tupla
colores = ("rojo", "verde", "azul")
print(colores)  
# Salida: ('rojo', 'verde', 'azul')

# Intentar cambiar un elemento (esto causará un error)
# colores[1] = "amarillo"  # TypeError: 'tuple' object does not support item assignment

# Acceder a un elemento por su índice
print("Elemento en el índice 1:", colores[1])  
# Salida: verde

# Intentar cambiar un elemento (esto causará un error)
# colores[1] = "amarillo"  # TypeError: 'tuple' object does not support item assignment

# Crear una nueva tupla combinando dos tuplas
nuevos_colores = colores + ("amarillo", "morado")
print("Tupla combinada:", nuevos_colores)  
# Salida: ('rojo', 'verde', 'azul', 'amarillo', 'morado')

# Repetir elementos de una tupla
repetidos = colores * 2
print("Tupla repetida:", repetidos)  
# Salida: ('rojo', 'verde', 'azul', 'rojo', 'verde', 'azul')

# Verificar si un elemento está en la tupla
existe = "verde" in colores
print("¿'verde' está en la tupla?:", existe)  
# Salida: True

# Obtener la longitud de la tupla
longitud = len(colores)
print("Longitud de la tupla:", longitud)  
# Salida: 3

# Contar el número de veces que un elemento aparece en la tupla
conteo = colores.count("rojo")
print("Conteo de 'rojo':", conteo)  
# Salida: 1

# Obtener el índice del primer elemento con el valor especificado
indice = colores.index("azul")
print("Índice de 'azul':", indice)  
# Salida: 2

# Desempaquetar una tupla en variables individuales
rojo, verde, azul = colores
print("Desempaquetado:", rojo, verde, azul)  
# Salida: rojo verde azul

# Desempaquetar una tupla con más elementos en una lista
colores_extendidos = ("rojo", "verde", "azul", "amarillo", "morado")
rojo, verde, azul, *otros_colores = colores_extendidos
print("Desempaquetado extendido:", rojo, verde, azul, otros_colores)  
# Salida: rojo verde azul ['amarillo', 'morado']
```

## Set

Los sets son colecciones desordenadas de elementos únicos. Se definen utilizando llaves {}.

```python
# Crear un set inicial
numeros = {1, 2, 3, 4, 5}
print("Set inicial:", numeros)  
# Salida: {1, 2, 3, 4, 5}

# Añadir un elemento al set
numeros.add(6)
print("Después de add:", numeros)  
# Salida: {1, 2, 3, 4, 5, 6}

# Intentar añadir un elemento duplicado (no tendrá efecto)
numeros.add(3)
print("Después de añadir un duplicado:", numeros)  
# Salida: {1, 2, 3, 4, 5, 6}

# Eliminar un elemento del set
numeros.remove(4)
print("Después de remove:", numeros)  
# Salida: {1, 2, 3, 5, 6}

# Eliminar un elemento con discard (no causa error si el elemento no existe)
numeros.discard(10)
print("Después de discard:", numeros)  
# Salida: {1, 2, 3, 5, 6}

# Eliminar y devolver un elemento aleatorio del set
elemento_eliminado = numeros.pop()
print("Elemento eliminado con pop:", elemento_eliminado)
print("Después de pop:", numeros)

# Limpiar todos los elementos del set
numeros.clear()
print("Después de clear:", numeros)  
# Salida: set()

# Crear dos sets para operaciones de conjuntos
set_a = {1, 2, 3}
set_b = {3, 4, 5}

# Unión de dos sets
union = set_a.union(set_b)
print("Unión de set_a y set_b:", union)  
# Salida: {1, 2, 3, 4, 5}

# Intersección de dos sets
interseccion = set_a.intersection(set_b)
print("Intersección de set_a y set_b:", interseccion)  
# Salida: {3}

# Diferencia de dos sets
diferencia = set_a.difference(set_b)
print("Diferencia de set_a y set_b:", diferencia)  
# Salida: {1, 2}

# Diferencia simétrica de dos sets
diferencia_simetrica = set_a.symmetric_difference(set_b)
print("Diferencia simétrica de set_a y set_b:", diferencia_simetrica)  
# Salida: {1, 2, 4, 5}

# Verificar si un set es subconjunto de otro
es_subconjunto = set_a.issubset({1, 2, 3, 4, 5})
print("¿set_a es subconjunto?:", es_subconjunto)  
# Salida: True

# Verificar si un set es superconjunto de otro
es_superconjunto = set_a.issuperset({1, 2})
print("¿set_a es superconjunto?:", es_superconjunto)  
# Salida: True

# Verificar si dos sets son disjuntos
son_disjuntos = set_a.isdisjoint({4, 5, 6})
print("¿set_a y {4, 5, 6} son disjuntos?:", son_disjuntos)  
# Salida: True
```

## Diccionarios

Los diccionarios son colecciones desordenadas de pares clave-valor. Se definen utilizando llaves {} y los pares clave-valor se separan con dos puntos :.

```python
# Crear un diccionario inicial
estudiante = {
    "nombre": "Juan",
    "edad": 21,
    "carrera": "Ingeniería"
}
print("Diccionario inicial:", estudiante)  
# Salida: {'nombre': 'Juan', 'edad': 21, 'carrera': 'Ingeniería'}

# Acceder a un valor por su clave
print("Nombre del estudiante:", estudiante["nombre"])  
# Salida: Juan

# Añadir un nuevo par clave-valor
estudiante["promedio"] = 8.5
print("Después de añadir 'promedio':", estudiante)  
# Salida: {'nombre': 'Juan', 'edad': 21, 'carrera': 'Ingeniería', 'promedio': 8.5}

# Modificar un valor existente
estudiante["edad"] = 22
print("Después de modificar 'edad':", estudiante)  
# Salida: {'nombre': 'Juan', 'edad': 22, 'carrera': 'Ingeniería', 'promedio': 8.5}

# Eliminar un par clave-valor con pop
promedio = estudiante.pop("promedio")
print("Valor eliminado con pop:", promedio)  
# Salida: 8.5
print("Después de pop:", estudiante)  
# Salida: {'nombre': 'Juan', 'edad': 22, 'carrera': 'Ingeniería'}

# Eliminar el último par clave-valor añadido con popitem
ultimo_elemento = estudiante.popitem()
print("Último elemento eliminado con popitem:", ultimo_elemento)  
# Salida: ('carrera', 'Ingeniería')
print("Después de popitem:", estudiante)  
# Salida: {'nombre': 'Juan', 'edad': 22}

# Obtener un valor con get
edad = estudiante.get("edad")
print("Edad del estudiante con get:", edad)  
# Salida: 22

# Intentar obtener un valor que no existe con get (devuelve None)
promedio = estudiante.get("promedio")
print("Promedio del estudiante con get:", promedio)  
# Salida: None

# Verificar si una clave existe en el diccionario
existe_nombre = "nombre" in estudiante
print("¿'nombre' está en el diccionario?:", existe_nombre)  
# Salida: True

# Obtener todas las claves del diccionario
claves = estudiante.keys()
print("Claves del diccionario:", claves)  
# Salida: dict_keys(['nombre', 'edad'])

# Obtener todos los valores del diccionario
valores = estudiante.values()
print("Valores del diccionario:", valores)  
# Salida: dict_values(['Juan', 22])

# Obtener todos los pares clave-valor del diccionario
items = estudiante.items()
print("Pares clave-valor del diccionario:", items)  
# Salida: dict_items([('nombre', 'Juan'), ('edad', 22)])

# Actualizar el diccionario con otro diccionario
estudiante.update({"carrera": "Matemáticas", "promedio": 9.0})
print("Después de update:", estudiante)  
# Salida: {'nombre': 'Juan', 'edad': 22, 'carrera': 'Matemáticas', 'promedio': 9.0}

# Limpiar todos los elementos del diccionario
estudiante.clear()
print("Después de clear:", estudiante)  
# Salida: {}
```
