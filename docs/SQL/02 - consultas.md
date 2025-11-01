# SQL - Consulta de Datos (DQL)

Los componentes `SELECT` y `FROM` son la base para construir la consulta más simple (query) en SQL, formando parte del Lenguaje de Consulta de Datos (DQL).

La función de una consulta simple es leer o recuperar datos de la base de datos sin modificar el contenido ni la estructura de las tablas.

## SELECT (Seleccionar Columnas)

La cláusula `SELECT` se utiliza para especificar qué columnas (o campos) se desean recuperar y mostrar en el resultado de la consulta.

- Seleccionar Todas las Columnas (SELECT *):
  - El asterisco (*) se utiliza como un atajo para indicar que se deben recuperar todas las columnas de la tabla especificada.
  - Ejecutar `SELECT *` permite ver "todo" lo que está dentro de la tabla, incluyendo todas las filas y columnas.
- Seleccionar Columnas Específicas:
  - Para ser más específico, en lugar del asterisco, se debe proporcionar una lista de las columnas deseadas, separadas por una coma.
  - Las columnas deben listarse exactamente después de la palabra clave `SELECT`.
  - Cuando se listan columnas específicas, cualquier columna que no esté mencionada en la lista de `SELECT` será excluida del resultado final.
  - Nota importante: No debe haber una coma después del nombre de la última columna en la lista, ya que SQL esperaría otra columna y, si encuentra la cláusula FROM inmediatamente después, devolverá un error.
  - Ejemplo: `SELECT nombre, apellido, edad`

## FROM (Especificar la Tabla)

La cláusula `FROM` es necesaria para indicarle a SQL dónde tiene que buscar los datos.

- Uso: Inmediatamente después de la palabra clave `FROM`, se debe especificar el nombre de la tabla que contiene la información que se desea consultar.
- El nombre de la tabla debe escribirse exactamente como está definido en la base de datos.

## WHERE (Filtrar Datos)

La cláusula `WHERE` se utiliza para filtrar los resultados de una consulta, permitiendo seleccionar solo aquellas filas que cumplen con ciertas condiciones.

- Posición y Orden de Ejecución: La cláusula `WHERE` se define después de la cláusula `FROM` en la sintaxis de la consulta. Al ejecutar, el motor de SQL primero recupera los datos de la tabla especificada en `FROM` y luego ejecuta el `WHERE` para aplicar el filtro.
- Condiciones: La condición dentro de `WHERE` puede ser cualquier expresión lógica. SQL evalúa esta condición fila por fila; si la condición es verdadera (True), la fila se mantiene en el resultado, pero si es falsa (False), la fila se elimina.
- Tipos de Valores:
  - Si el valor que se utiliza para la comparación contiene caracteres (strings), debe ser escrito entre comillas simples (single quotes).
  - Si el valor es numérico (entero, decimal, etc.), puede escribirse sin comillas simples.

### WHERE vs. HAVING (Filtro de Agregación)

Es crucial distinguir la cláusula `WHERE` de la cláusula `HAVING`:

- `WHERE`: Filtra los datos antes de que se realicen las operaciones de agregación (como SUM, AVG o COUNT) o la agrupación de datos (GROUP BY).
- `HAVING`: Se utiliza para filtrar los resultados después de que la agregación ha ocurrido.

### Uso en Manipulación y Optimización

- Manipulación de Datos (DML): Es una práctica obligatoria usar la cláusula `WHERE` con los comandos `UPDATE` o `DELETE`. Si se omite, la operación afectaría a todas las filas de la tabla.
- Buenas Prácticas: Para asegurar el rendimiento óptimo y permitir que el motor de SQL utilice los índices, se recomienda evitar el uso de funciones en la columna de la cláusula `WHERE`.

### Ejemplos

```sql
SELECT nombre, apellido, edad FROM empleados
WHERE edad > 18;
```
```sql
SELECT *
FROM Customers
WHERE Country = 'Germany';
```

### DISTINCT (Eliminar Duplicados)

La cláusula `DISTINCT` se utiliza para eliminar duplicados de los resultados de una consulta.

- Uso: Se coloca después de la cláusula `SELECT` para indicar que solo se desean mostrar valores únicos en el resultado.
- Ejemplo: `SELECT DISTINCT nombre FROM empleados` devolverá una lista de nombres únicos, eliminando cualquier nombre repetido.

### Ejemplos

```sql
SELECT DISTINCT nombre FROM empleados;
```
```sql
SELECT DISTINCT nombre, apellido FROM empleados;
```

## TOP (Limitar Resultados)

La cláusula `TOP` se utiliza para limitar el número de filas devueltas por una consulta.

- Uso: Se coloca después de la cláusula `SELECT` para indicar cuántas filas se desean mostrar en el resultado.
- Ejemplo: `SELECT TOP 10 nombre FROM empleados` devolverá solo las primeras 10 filas de la tabla.

### Ejemplos

```sql
SELECT TOP 10 nombre FROM empleados;
```
```sql
SELECT TOP 5 nombre, apellido FROM empleados;
```

## LIMIT (Limitar Resultados)

La cláusula `LIMIT` se utiliza para limitar el número de filas devueltas por una consulta.

> Nota: Esta cláusula es específica de MySQL y no es estándar.

- Uso: Se coloca después de la cláusula `SELECT` para indicar cuántas filas se desean mostrar en el resultado.
- Ejemplo: `SELECT LIMIT 10 nombre FROM empleados` devolverá solo las primeras 10 filas de la tabla.

### Ejemplos

```sql
SELECT
  employee_id,
  first_name,
  last_name
FROM
  employees
LIMIT
  5;
```

## Orden y Ejecución de la Consulta

La sintaxis de la consulta más sencilla siempre comienza con `SELECT` y le sigue `FROM`.

1. Orden de Codificación (lo que se escribe):
    ```sql
    SELECT nombre, apellido, edad FROM empleados;
    ```
    o
    ```sql
    SELECT nombre, apellido, edad
    FROM empleados;
    ```
2. Orden de Ejecución (lo que hace SQL):
    - SQL comienza ejecutando primero la cláusula `FROM` para ir a la base de datos y recuperar toda la información de la tabla especificada.
    - Luego, ejecuta la cláusula `SELECT` para determinar qué columnas deben mantenerse y presentarse en el resultado final.