# Bucles

Los bucles permiten repetir un bloque de código varias veces.

## ➡️ Bucle for

```python
for i in range(5):  # Itera sobre una secuencia de números del 0 al 4
    print(i)
```

## ➡️ Bucle while

```python
contador = 0
while contador < 5:
    print(contador)
    contador += 1
```

## ➡️ Control de bucles

Estos comandos permiten modificar el flujo de los bucles.

- **break**: Sale del bucle.

```python
for i in range(10):
    if i == 5:
        break
    print(i)
# Imprime: 0, 1, 2, 3, 4
```

- **continue**: Salta la iteración actual y continúa con la siguiente.

```python
for i in range(5):
    if i == 3:
        continue
    print(i)
# Imprime: 0, 1, 2, 4
```

- **pass**: No hace nada, se usa como marcador de posición.

```python
for i in range(5):
    if i == 2:
        pass  # Aquí no hacemos nada
    print(i)
# Imprime: 0, 1, 2, 3, 4
```
