# SQL - Manipulación de Datos (DML)

Conjunto de comandos SQL dedicados a **manipular los datos** que ya se encuentran dentro de la base de datos.

## INSERT

Sirve para añadir nuevos registros (filas) a una tabla.

```sql
INSERT INTO clientes (nombre, email)
VALUES ('Ana Ruiz', 'ana@email.com');
```

### Insertar múltiples registros

```sql
INSERT INTO clientes (nombre, email)
VALUES 
    ('Juan Pérez', 'juan@email.com'),
    ('María López', 'maria@email.com');
```

### Insertar datos de otra tabla

```sql
INSERT INTO clientes_nuevos (nombre, email)
SELECT nombre, email FROM clientes WHERE ciudad = 'Madrid';
```

## UPDATE

Se utiliza para modificar los datos de uno o varios registros existentes en una tabla.

```sql
UPDATE clientes
SET email = 'ana.nuevo@email.com'
WHERE nombre = 'Ana Ruiz';
```

Esto modifica el email solo del cliente llamado "Ana Ruiz".​

> **¡IMPORTANTE! Si olvidas el WHERE, actualizarás **Todos los registros** de la tabla.**

## DELETE

Se emplea para eliminar registros de una tabla, pudiendo borrar una, varias o todas las filas.

```sql
DELETE FROM clientes WHERE nombre = 'Ana Ruiz';
```
Esto eliminará el cliente con ese correo electrónico.​

> **¡IMPORTANTE! Si olvidas el WHERE, borrarás **Todos los registros** de la tabla.**
