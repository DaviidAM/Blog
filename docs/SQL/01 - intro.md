# SQL - Introducción y Estructura

SQL (Structured Query Language) es el lenguaje que se usa para hablar con las bases de datos.

## Definiciones básicas dentro de SQL

### Base de Datos (Database)

Una base de datos es esencialmente un contenedor para almacenar datos, agrupados de forma organizada y estructurada.

Puedes imaginarla como una carpeta grande donde se guarda todo lo relacionado con una aplicación o sistema.

> Ejemplo: Si tienes una empresa, puedes tener una base de datos llamada `EmpresaDB` donde se almacena la información de empleados, productos, ventas, etc.​

#### Beneficios

- Manejo de grandes volúmenes de datos: Pueden gestionar enormes cantidades de datos (a veces millones), algo que las hojas de cálculo simples no pueden manejar sin romperse.

- Eficiencia en consultas: Permiten "hablar" a los datos para hacer preguntas y obtener resultados de manera rápida y sencilla, un proceso mucho más rápido que buscar manualmente en archivos dispersos.

- Seguridad: Son más seguras para almacenar datos importantes y permiten controlar quién accede a qué.

### Servidor (Server) y DBMS

Aunque la base de datos es el contenedor de los datos, necesita tanto hardware como software para funcionar y ser accesible:

- Servidor: Es la máquina física, o una PC muy potente, donde reside la base de datos. El servidor es necesario porque las computadoras personales suelen ser débiles y no están disponibles 24/7. Un servidor potente está siempre disponible (24/7).

- Sistema de Gestión de Bases de Datos (DBMS): Es un software que actúa como el administrador de la base de datos. El DBMS se encarga de gestionar todas las solicitudes (consultas SQL) enviadas a la base de datos, priorizar qué SQL debe ejecutarse primero, y manejar la seguridad.

### Esquema (Schema)

El esquema se encuentra dentro de la base de datos y representa el siguiente nivel de organización.

Un esquema es un contenedor lógico dentro de una base de datos que agrupa objetos relacionados, como tablas, vistas y procedimientos.

Sirve para organizar los objetos dentro de la base de datos y, además, facilitar la gestión de permisos o separar lógicamente diferentes partes de una aplicación.

Un esquema se puede comparar con una sub-carpeta dentro de la carpeta principal (la base de datos).

> Ejemplo: Dentro de la base de datos `EmpresaDB` puedes tener un esquema llamado `recursos_humanos` que agrupe objetos sólo de ese departamento, y otro esquema llamado `ventas` para las tablas de ventas.​

### Tablas (Tables)

La tabla es el objeto fundamental para almacenar datos dentro de la base de datos. Se la describe como:

- Una colección estructurada de datos.
- Similar a una hoja de cálculo simple o una cuadrícula (como en Excel).
- Organiza los datos en columnas.
- Contiene múltiples columnas y filas.
- Es un objeto que se encuentra dentro de un esquema (Schema).

Las tablas se almacenan físicamente en archivos de base de datos en el almacenamiento en disco, aunque para el usuario se representan como una abstracción de filas y columnas.

Cada tabla está dentro de un esquema, y cada esquema está dentro de una base de datos.

> Ejemplo: Dentro del esquema `recursos_humanos` puedes tener una tabla llamada `empleados` con columnas como `id_empleado`, `nombre`, y `fecha_ingreso`.

### Columnas (Campos / Columns, Fields)

Las columnas definen el tipo de datos que se almacenan. Son los atributos o características del objeto que describe la tabla.

- Cada columna trata sobre un tipo de dato.
- Definen la estructura de la tabla.
- Ejemplos de columnas incluyen el ID del cliente, los nombres, las puntuaciones o las fechas de cumpleaños.
- A veces se les llama campos (fields).
- Cada columna tiene un nombre y un tipo de dato.
- Ejemplos de tipos de datos incluyen INT, DECIMAL, CHAR o VARCHAR.

### Filas (Registros / Rows, Records)

Las filas, también llamadas registros (records), son donde se almacenan los datos reales.

- Cada fila representa una entrada o registro completo de datos.
- Por ejemplo, en una tabla de clientes, cada registro (fila) representa a un cliente individual (como Maria, John o Peter).

### Celdas (Cells)

La celda es la intersección o el solapamiento (overlapping) entre una columna y una fila.

- Una celda contiene una pieza individual de datos.
- Es un valor único.
- Cada valor dentro de la celda debe almacenar un tipo de dato específico.

## Ejemplo práctico

```sql
-- Crear una base de datos
CREATE DATABASE EmpresaDB;

-- Usar la base de datos
USE EmpresaDB;

-- Crear un esquema
CREATE SCHEMA recursos_humanos;

-- Crear una tabla dentro del esquema
CREATE TABLE recursos_humanos.empleados (
  id_empleado INT PRIMARY KEY,       -- Columna: identifica el id (cada fila tendrá un valor para esta columna)
  nombre VARCHAR(100),               -- Columna: almacena el nombre del empleado
  fecha_ingreso DATE                 -- Columna: almacena la fecha de ingreso del empleado
);
```