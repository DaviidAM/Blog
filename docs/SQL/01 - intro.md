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

## Tipos de datos

### INT (Integer)

El tipo de dato `INT` se utiliza para almacenar números enteros.

- Uso: Se define para una columna cuyo contenido es exclusivamente numérico y no contiene caracteres.
- Ejemplos: Valores como 1, 2 o 30 son ejemplos de números enteros que pueden almacenarse como INT.
- Contexto: Cuando se definen columnas como INT, esto ayuda a evitar problemas de rendimiento y asegura que se utilice el tipo de dato más preciso para el contenido.

### DECIMAL

El tipo de dato `DECIMAL` se utiliza para almacenar números que contienen un punto decimal. Almacena números con precisión exacta y escala fija, ideal para valores monetarios o cantidades exactas.

• Ejemplo: Un valor como 3.14 se almacenaría como DECIMAL.

### CHAR y VARCHAR (Tipos de Caracteres/Cadenas)

Estos tipos de datos se utilizan para almacenar caracteres (strings), como nombres, descripciones o cualquier valor que contenga texto.

#### CHAR (Longitud Fija)

El tipo de dato `CHAR` siempre tiene una longitud fija. Almacena cadenas de longitud variable con caracteres ASCII no Unicode, usa 1 byte por carácter, eficiente para datos en idiomas que no necesitan Unicode.

- Comportamiento: Si se define un campo como CHAR con una longitud de, por ejemplo, cinco caracteres, el sistema siempre irá y reservará ese espacio de cinco caracteres para el almacenamiento, independientemente de la longitud real del valor insertado.

#### VARCHAR (Longitud Variable)

El tipo de dato `VARCHAR` (también conocido como VCHAR) es más dinámico en comparación con CHAR.

- Uso: Se utiliza para columnas que contienen caracteres, como un nombre de persona o un número de teléfono. Al definir una columna como VARCHAR, debe especificarse la longitud máxima, por ejemplo, VARCHAR(50).

### Tipos de Datos de Fecha y Hora (Date and Time Types)

Estos tipos se utilizan para almacenar valores que representan momentos específicos:

- DATE (Fecha): Se utiliza para almacenar información de fecha. Una fecha generalmente se compone del año, el mes y el día.
- TIME (Tiempo): Se utiliza para almacenar información de tiempo. Esto incluye componentes como horas, minutos y segundos.
- DATETIME o TIMESTAMP: Este tipo combina la información de fecha junto con la hora. El término TIMESTAMP se usa comúnmente en bases de datos como Oracle, Postgress y MySQL, mientras que en SQL Server se le llama DATETIME.
  - Soporta fechas desde el 1 de enero de 1753 hasta el 31 de diciembre de 9999.
- DATETIME2: Este es un formato específico que aparece en las definiciones de columnas, como en la capa silver de un almacén de datos.
  - Soporta fechas desde el 1 de enero del año 1 hasta el 31 de diciembre de 9999.

> Nota: Las funciones de SQL que extraen partes de las fechas, como DAY, MONTH, YEAR o DATEPART, devuelven un valor de tipo INTEGER.

### Tipos Numéricos Adicionales

Además de los enteros (INT) y decimales (DECIMAL), existen variantes numéricas:

- FLOAT: Se utiliza para variables y valores numéricos que contienen puntos decimales y que necesitan una precisión flotante.
  - Almacena números en forma de punto flotante con precisión variable y puede perder precisión en cálculos complejos, siendo más adecuado para mediciones científicas o valores aproximados. 
- INTEGER: Se confirma su uso al definir variables para almacenar recuentos, como el total de clientes.
Generalmente, las funciones de agregación como SUM, AVG, MIN y MAX requieren que la expresión o el argumento que se les pasa sea de un tipo de dato numérico.
  - Tiene prácticamente el mismo uso que `INT`, pero con la diferencia de que puede almacenar números con decimales.

### Tipos de Caracteres Extendidos

Si bien CHAR y VARCHAR son los más comunes, existen otros tipos para manejar cadenas de texto:
- TEXT: Este tipo se utiliza para almacenar cadenas de caracteres. Sin embargo, se recomienda evitar su uso, junto con VARCHAR, ya que pueden consumir muchos recursos y generar problemas de rendimiento y fragmentación de datos, por lo que es mejor si es posible utilizar un tipo de dato más preciso.
  - Ya obsoleto en favor de VARCHAR(MAX), ideal para textos muy largos que no tienen longitud fija.
- NVARCHAR: Almacena cadenas de longitud variable con soporte Unicode (como caracteres especiales o de otros idiomas), usando 2 bytes por carácter, ideal para internacionalización pero ocupa más espacio

### El Concepto de NULL

Es importante destacar que el valor NULL no es un tipo de dato en sí mismo.

- Significado: Un valor NULL en una base de datos significa desconocido o faltante.
- Diferenciación: NULL es diferente de cero (0), una cadena vacía ('') o un espacio en blanco (' '). Un valor NULL no tiene un tipo de dato asociado, sino que es un marcador especial.

## Clave primaria

La **Clave Primaria** (*Primary Key*) es un concepto fundamental en la definición y la gestión de tablas en SQL, ya que define el identificador único de cada registro.

A continuación, se detalla su rol principal y sus funciones esenciales.

### Rol como Identificador Único (Unicidad)

La clave primaria es una columna muy importante en cada tabla. Su rol principal es proveer un identificador único para cada fila o cliente.

- Naturaleza Única: La clave primaria es inherentemente única. Se la describe como una "huella dactilar" (fingerprint) que garantiza que no haya dos clientes que compartan el mismo ID.
- Integridad de Datos: Una de las razones por las que se establece una clave primaria es para garantizar la integridad de la tabla. Al definir una columna como clave primaria, se espera que sus valores sean únicos y no contengan duplicados. Si se comprueba su unicidad mediante un conteo, el resultado de cada valor debería ser, como máximo, uno.

### Soporte para Conexiones y Relaciones

La clave primaria es esencial para la estructura relacional de la base de datos:

- Conectabilidad: Su existencia permite que la tabla sea "conectable" a otras tablas.
- Combinación de Tablas: Se utiliza para combinar la tabla con otra tabla y para identificar rápidamente un cliente. En sistemas relacionales, la clave primaria define las relaciones (Primary Key y Foreign Key) necesarias para unir datos de múltiples tablas.

### Optimización y Rendimiento

La clave primaria tiene un impacto directo en el rendimiento de la base de datos:

- Candidato Ideal para Índices Agrupados (Clustered Index): La clave primaria es un candidato perfecto para crear un índice agrupado. Esto se debe a que cumple con los criterios de ser única y, típicamente, sus valores no se modifican con frecuencia (solo se añaden nuevos valores), lo cual evita operaciones costosas de reordenamiento de datos.
- Mejora del Rendimiento: Tener una clave primaria en todas las tablas es una buena práctica, ya que mejora la velocidad de búsqueda (lookups) cuando se realizan operaciones de modificación, como UPDATE o DELETE.

## Comentarios

Los comentarios en SQL son una forma de agregar notas o explicaciones dentro del código, que no se ejecutan como parte de la consulta. Son útiles para documentar el propósito de ciertas secciones del código, explicar la lógica detrás de ciertas operaciones o para deshabilitar ciertas partes del código sin eliminarlas.

- Uso: Se comienza con dos guiones (--) para comentarios de una línea o con /* */ para comentarios de varias líneas.

### Ejemplos

```sql
-- Este es un comentario de una línea
SELECT nombre, apellido, edad FROM empleados;
```
```sql
/*
Este es un comentario de varias líneas
que se extiende por varias líneas
*/
SELECT nombre, apellido, edad FROM empleados;
```