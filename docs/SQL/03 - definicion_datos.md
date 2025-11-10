# SQL - Definición de Datos (DDL)

Su propósito fundamental es **definir la estructura** y **crear** un objeto completamente nuevo dentro de la base de datos.

## CREATE

Es el comando que se utiliza para crear un objeto completamente nuevo dentro de la base de datos.

El comando `CREATE` se utiliza para definir diversos objetos de la base de datos, entre los que se incluyen:

1. **Tablas (CREATE TABLE)**: Crea una nueva tabla para almacenar datos.
2. **Vistas (CREATE VIEW)**: Crea una lógica de consulta almacenada, que actúa como una tabla virtual para abstracción o seguridad.
3. **Bases de Datos (CREATE DATABASE)**: Se utiliza para iniciar una nueva base de datos.
4. **Esquemas (CREATE SCHEMA)**: Crea un contenedor lógico para organizar objetos dentro de una base de datos.

### CREATE DATABASE

Se usa `CREATE DATABASE` para crear una nueva base de datos vacía.

```sql
CREATE DATABASE tienda;
```

### CREATE TABLE

CREATE TABLE permite definir una nueva tabla y sus columnas. Debes indicar el nombre, las columnas y su tipo de dato.

```sql
CREATE TABLE clientes  (
  id INT PRIMARY KEY,
  nombre VARCHAR(100),
  email VARCHAR(100)
);
```

Ejemplo con más detalles y restricciones
```sql
CREATE TABLE pedidos (
    id INT AUTO_INCREMENT,
    fecha DATE,
    cliente_id INT,
    PRIMARY KEY (id),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

#### Crear tabla desde otra tabla

Puedes crear una tabla nueva copiando los datos de otra tabla existente:

```sql
CREATE TABLE nuevos_clientes AS
SELECT * FROM clientes WHERE ciudad = 'Madrid';
```

Esto crea la tabla "nuevos_clientes" con solo los clientes de Madrid.​

### CREATE INDEX

CREATE INDEX se utiliza para crear un índice en una tabla, lo que mejora el rendimiento de las consultas.

```sql
CREATE INDEX idx_nombre ON clientes(nombre);
```

Crea un índice llamado "idx_nombre" sobre la columna "nombre" de la tabla "clientes".

### CREATE VIEW

CREATE VIEW se utiliza para crear una vista, que es una tabla virtual que contiene una consulta almacenada.

```sql
CREATE VIEW clientes_madrid AS
SELECT * FROM clientes WHERE ciudad = 'Madrid';
```

## ALTER

`ALTER` se utiliza para modificar la estructura de una tabla existente.

### Añadir columna

```sql
ALTER TABLE clientes
ADD fecha_registro DATE;
```

Agrega una columna llamada fecha_registro a la tabla "clientes".

### Modificar columna

```sql
ALTER TABLE clientes
ALTER COLUMN nombre TYPE VARCHAR(150);
```

Modifica la columna "nombre" de la tabla "clientes", cambiando su tipo de dato a VARCHAR(200).

### Eliminar columna

```sql
ALTER TABLE clientes
DROP COLUMN fecha_registro;
```

Elimina la columna fecha_registro de la tabla "clientes".

### Modificar nombre de la tabla

```sql
ALTER TABLE clientes
RENAME TO clientes_nuevos;
```

Modifica el nombre de la tabla "clientes" a "clientes_nuevos".

### Añadir restricción

```sql
ALTER TABLE clientes
ADD CONSTRAINT uq_email UNIQUE (email);
```

Agrega una restricción llamada "uq_email" que verifica que el email sea único.

## DROP

`DROP` se utiliza para eliminar objetos de la base de datos.

### Eliminar tabla

```sql
DROP TABLE clientes;
```

Elimina la tabla "clientes" de la base de datos.

### Eliminar base de datos

```sql
DROP DATABASE tienda;
```

Elimina la base de datos "tienda".

