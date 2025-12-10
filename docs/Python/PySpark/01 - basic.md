# PySpark Básico

**PySpark** es una biblioteca de Python de código abierto, ampliamente utilizada en ciencia de datos y machine learning. Proporciona estructuras de datos flexibles y eficientes diseñadas específicamente para el procesamiento de grandes volúmenes de información.

## Configuración Inicial de Spark
- Cada proyecto en PySpark comienza con la creación de una `SparkSession`. Es una buena práctica asignar un nombre descriptivo (como *pyspark_test_basic_operations*) para facilitar el seguimiento en la interfaz de usuario de Spark.
- La configuración sensible se maneja mediante variables de entorno usando `python-dotenv`, manteniendo las credenciales seguras en un archivo `.env`.
```python
from pyspark.sql import SparkSession
from dotenv import load_dotenv

load_dotenv()

spark = SparkSession.builder.appName("pyspark_test_basic_operations").getOrCreate()
```

## Carga de Datos de Ejemplo
- El primer paso es cargar los datos en un **DataFrame**, que es la estructura fundamental en PySpark.
    - En este ejemplo, trabajaremos con un archivo CSV ubicado en la carpeta `resources/`.
    - Parámetros importantes al leer el archivo:
      - `header=True`: Mantiene los nombres de las columnas del archivo CSV.
      - `inferSchema=True`: Detecta automáticamente los tipos de datos (números, cadenas, etc.) de cada columna, lo que acelera el proceso de prototipado.

```python
df = spark.read.csv("resources/test1.csv", header=True, inferSchema=True)
```

## Exploración y Transformación de DataFrames
### Análisis Inicial
- `show()`: Muestra una vista previa de los datos.
- `printSchema()`: Muestra la estructura y tipos de datos.
- `columns`: Lista los nombres de las columnas.
- `count()`: Cuenta el número total de filas.
- `head()`: Muestra las primeras filas del conjunto de datos.

### Operaciones Comunes
- **Selección de columnas**: Extraer columnas específicas del DataFrame.
- `describe()`: Proporciona estadísticas descriptivas (media, mínimo, máximo, desviación estándar).
- **Creación de columnas**: Se pueden crear nuevas columnas basadas en operaciones con columnas existentes.

### Inmutabilidad
Las transformaciones en PySpark son inmutables, lo que significa que cada operación devuelve un nuevo DataFrame. Esto facilita el seguimiento de los cambios y evita efectos secundarios no deseados.

### Ejemplo de uso
```python
df.show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
+---------+---+----------+------+
'''

df.printSchema()
'''
root
|-- Name: string (nullable = true)
|-- age: integer (nullable = true)
|-- Experience: integer (nullable = true)
|-- Salary: integer (nullable = true)
'''

print(f"Columns -> {df.columns}")
# Columns -> ['Name', 'age', 'Experience', 'Salary']

print(f"Number of rows -> {df.count()}")
# Number of rows -> 6

print(df.head(2))
'''
[Row(Name='Krish', age=31, Experience=10, Salary=30000),
Row(Name='Sudhanshu', age=30, Experience=8, Salary=25000)]
'''
print(df.count())

df.select("Name", "Experience").show()
'''
+---------+----------+
|     Name|Experience|
+---------+----------+
|    Krish|        10|
|Sudhanshu|         8|
|    Sunny|         4|
|     Paul|         3|
|   Harsha|         1|
|  Shubham|         2|
+---------+----------+
'''

df.describe().show()
'''
+-------+------+------+-----------+----------+
|summary|  Name|   age| Experience|    Salary|
+-------+------+------+-----------+----------+
|  count|     6|     6|          6|         6|
|   mean|  NULL| 26.33|      4.667| 21333.332|
| stddev|  NULL|  4.18|      3.559|  5354.126|
|    min|Harsha|    21|          1|     15000|
|    max| Sunny|    31|         10|     30000|
+-------+------+------+-----------+----------+
'''

df = df.withColumn("Experience_in_2_years", df.Experience + 2)
'''
+---------+---+----------+------+---------------------+
|     Name|age|Experience|Salary|Experience in 2 years|
+---------+---+----------+------+---------------------+
|    Krish| 31|        10| 30000|                   12|
|Sudhanshu| 30|         8| 25000|                   10|
|    Sunny| 29|         4| 20000|                    6|
|     Paul| 24|         3| 20000|                    5|
|   Harsha| 21|         1| 15000|                    3|
|  Shubham| 23|         2| 18000|                    4|
+---------+---+----------+------+---------------------+
'''

df.drop("Experience in 2 years").show()

df = df.withColumnRenamed("Experience", "Exp")
```

## Manejo de Valores Faltantes
### Eliminación de Datos Faltantes
- `df.na.drop()`: Elimina filas con valores nulos.
  - Sin parámetros: Elimina filas con cualquier valor nulo.
  - `subset`: Especifica columnas específicas para verificar valores nulos.
  - `thresh`: Mantiene filas con un mínimo de valores no nulos.
  - `how`: 'any' (por defecto) o 'all' para eliminar filas con algún o todos los valores nulos.

### Imputación de Valores
- `df.na.fill()`: Rellena valores nulos con valores específicos.
  - Se puede usar con valores escalares o diccionarios para diferentes columnas.
- `Imputer` de `pyspark.ml.feature`: 
  - Calcula automáticamente estadísticas (como la mediana).
  - Crea nuevas columnas con el sufijo '_imputed'.
  - Permite diferentes estrategias de imputación (media, mediana, moda).

### Ejemplo de uso
```python
df.show()
'''
+---------+----+----------+------+
|     Name| age|Experience|Salary|
+---------+----+----------+------+
|    Krish|  31|        10| 30000|
|Sudhanshu|  30|         8| 25000|
|    Sunny|  29|         4| 20000|
|     Paul|  24|         3| 20000|
|   Harsha|  21|         1| 15000|
|  Shubham|  23|         2| 18000|
|   Mahesh|NULL|      NULL| 40000|
|     NULL|  34|        10| 38000|
|     NULL|  36|      NULL|  NULL|
+---------+----+----------+------+
'''

#############
# Drop Null values
#############
# Remove all rows with null values
print(">> Remove all rows with null values")
df.na.drop().show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
+---------+---+----------+------+
'''

# Remove all rows with null values in a specific column
df.na.drop(subset=["Name"]).show()
'''
+---------+----+----------+------+
|     Name| age|Experience|Salary|
+---------+----+----------+------+
|    Krish|  31|        10| 30000|
|Sudhanshu|  30|         8| 25000|
|    Sunny|  29|         4| 20000|
|     Paul|  24|         3| 20000|
|   Harsha|  21|         1| 15000|
|  Shubham|  23|         2| 18000|
|   Mahesh|NULL|      NULL| 40000|
+---------+----+----------+------+
'''

# Remove all rows with null values
# how="any" means remove all rows with any null values (Default)
# how="all" means remove all rows with null values in all columns
df.na.drop(how="any").show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
+---------+---+----------+------+
'''

# Remove all rows with null values depending on threshold 
# thresh=2 means remove all rows with less than 2 NON NULL values
df.na.drop(thresh=2).show()
'''
+---------+----+----------+------+
|     Name| age|Experience|Salary|
+---------+----+----------+------+
|    Krish|  31|        10| 30000|
|Sudhanshu|  30|         8| 25000|
|    Sunny|  29|         4| 20000|
|     Paul|  24|         3| 20000|
|   Harsha|  21|         1| 15000|
|  Shubham|  23|         2| 18000|
|   Mahesh|NULL|      NULL| 40000|
|     NULL|  34|        10| 38000|
+---------+----+----------+------+
'''

# Remove all rows with null values depending on subset
# subset=["Name", "Experience"] means remove all rows with any null values in name or experience columns
df.na.drop(subset=["Name", "Experience"]).show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
+---------+---+----------+------+
'''

#############
# Fill Null values
#############

# Fill all null values with a specific value
df.na.fill("Not Found 1").show()
'''
+-----------+----+----------+------+
|       Name| age|Experience|Salary|
+-----------+----+----------+------+
|      Krish|  31|        10| 30000|
|  Sudhanshu|  30|         8| 25000|
|      Sunny|  29|         4| 20000|
|       Paul|  24|         3| 20000|
|     Harsha|  21|         1| 15000|
|    Shubham|  23|         2| 18000|
|     Mahesh|NULL|      NULL| 40000|
|Not Found 1|  34|        10| 38000|
|Not Found 1|  36|      NULL|  NULL|
+-----------+----+----------+------+
'''

# Fill all null values with a specific value in a specific column
# (not working with age column because it's a int column)
df.na.fill("Not Found 2", subset=["Name", "age"]).show()
'''
+-----------+----+----------+------+
|       Name| age|Experience|Salary|
+-----------+----+----------+------+
|      Krish|  31|        10| 30000|
|  Sudhanshu|  30|         8| 25000|
|      Sunny|  29|         4| 20000|
|       Paul|  24|         3| 20000|
|     Harsha|  21|         1| 15000|
|    Shubham|  23|         2| 18000|
|     Mahesh|NULL|      NULL| 40000|
|Not Found 2|  34|        10| 38000|
|Not Found 2|  36|      NULL|  NULL|
+-----------+----+----------+------+
'''

# working with age column because it's a int column, but not with name column because it's a string column
df.na.fill(0, subset=["Name", "age"]).show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
|   Mahesh|  0|      NULL| 40000|
|     NULL| 34|        10| 38000|
|     NULL| 36|      NULL|  NULL|
+---------+---+----------+------+
'''

# working with both columns using a dictionary
df.na.fill({"Name": "Not Found 2", "age": 0}).show()
'''
+-----------+---+----------+------+
|       Name|age|Experience|Salary|
+-----------+---+----------+------+
|      Krish| 31|        10| 30000|
|  Sudhanshu| 30|         8| 25000|
|      Sunny| 29|         4| 20000|
|       Paul| 24|         3| 20000|
|     Harsha| 21|         1| 15000|
|    Shubham| 23|         2| 18000|
|     Mahesh|  0|      NULL| 40000|
|Not Found 2| 34|        10| 38000|
|Not Found 2| 36|      NULL|  NULL|
+-----------+---+----------+------+
'''

# Impute null values with median of column
from pyspark.ml.feature import Imputer
imputer = Imputer(
    inputCols=['age', 'Experience', 'Salary'], 
    outputCols=["{}_imputed".format(c) for c in ['age', 'Experience', 'Salary']]
).setStrategy("median")
imputer.fit(df_pyspark).transform(df_pyspark).show()
'''
+---------+----+----------+------+-----------+------------------+--------------+
|     Name| age|Experience|Salary|age_imputed|Experience_imputed|Salary_imputed|
+---------+----+----------+------+-----------+------------------+--------------+
|    Krish|  31|        10| 30000|         31|                10|         30000|
|Sudhanshu|  30|         8| 25000|         30|                 8|         25000|
|    Sunny|  29|         4| 20000|         29|                 4|         20000|
|     Paul|  24|         3| 20000|         24|                 3|         20000|
|   Harsha|  21|         1| 15000|         21|                 1|         15000|
|  Shubham|  23|         2| 18000|         23|                 2|         18000|
|   Mahesh|NULL|      NULL| 40000|         29|                 4|         40000|
|     NULL|  34|        10| 38000|         34|                10|         38000|
|     NULL|  36|      NULL|  NULL|         36|                 4|         20000|
+---------+----+----------+------+-----------+------------------+--------------+
'''
```

## Técnicas de Filtrado de Filas
### Sintaxis de Filtrado
- **Estilo SQL**: Usando cadenas de texto, ej: `"Salary >= 25000"`
- **Expresiones de Columnas**: Usando operadores lógicos, ej: `(df.Salary >= 25000) | (df.Experience >= 3)`

### Buenas Prácticas
- Usar `.select()` después de filtrar para mostrar solo las columnas relevantes.
- Combinar múltiples condiciones usando operadores lógicos (`&` para AND, `|` para OR).
- Utilizar paréntesis para agrupar condiciones complejas y evitar ambigüedades.

### Ejemplo de uso
```python
df.show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
+---------+---+----------+------+
'''

df.filter("Salary >= 25000").select("Name", "age").show()
'''
+---------+---+
|     Name|age|
+---------+---+
|    Krish| 31|
|Sudhanshu| 30|
+---------+---+
'''

df.filter((df.Salary >= 25000) | (df.Exp >= 3)).show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
+---------+---+----------+------+
'''
```

## Agregaciones y Agrupaciones
### Operaciones Básicas
- `groupBy()`: Agrupa los datos según una o más columnas.
- Funciones de agregación comunes:
  - `sum()`: Suma de valores.
  - `count()`: Conteo de registros.
  - `max()`/`min()`: Valores máximos y mínimos.
  - `avg()`: Promedio de valores.

### Agregaciones Múltiples
- `agg()`: Permite realizar múltiples operaciones de agregación en una sola llamada.
- Útil para calcular varias métricas simultáneamente.
- Soporta funciones integradas y expresiones SQL personalizadas.

### Ejemplo de uso
```python
df.show()
'''
+---------+------------+------+
|     Name| Departments|salary|
+---------+------------+------+
|    Krish|Data Science| 10000|
|    Krish|         IOT|  5000|
|   Mahesh|    Big Data|  4000|
|    Krish|    Big Data|  4000|
|   Mahesh|Data Science|  3000|
|Sudhanshu|Data Science| 20000|
|Sudhanshu|         IOT| 10000|
|Sudhanshu|    Big Data|  5000|
|    Sunny|Data Science| 10000|
|    Sunny|    Big Data|  2000|
+---------+------------+------+
'''

df.groupBy("Name").sum("salary").show()
'''
+---------+-----------+
|     Name|sum(salary)|
+---------+-----------+
|Sudhanshu|      35000|
|    Sunny|      12000|
|    Krish|      19000|
|   Mahesh|       7000|
+---------+-----------+
'''

df.groupBy("Departments").count().show()
'''
+------------+-----+
| Departments|count|
+------------+-----+
|         IOT|    2|
|    Big Data|    4|
|Data Science|    4|
+------------+-----+
'''

df.agg(
    {"salary": "sum",
    "Departments": "count"}
).show()
'''
+------------------+-----------+
|count(Departments)|sum(salary)|
+------------------+-----------+
|                10|      73000|
+------------------+-----------+
'''
```

## Flujo de Regresión Lineal (Machine Learning)
- **Preparación de características**: Las columnas numéricas (`age`, `Experience`) se combinan en un único vector usando `VectorAssembler`, creando la columna "Independent Features" necesaria para el modelo.
- **División de datos**: El conjunto de datos se divide en subconjuntos de entrenamiento y prueba usando `randomSplit()`.
- **Entrenamiento del modelo**: Se utiliza `LinearRegression` especificando la columna de características y la columna objetivo (etiqueta).
- **Evaluación del modelo**: La calidad del modelo se evalúa mediante:
  - Error absoluto medio (MAE)
  - Error cuadrático medio (MSE)
  - Raíz del error cuadrático medio (RMSE)
  - Coeficiente de determinación (R²)

### Ejemplo de uso
```python
training.show()
'''
+---------+---+----------+------+
|     Name|age|Experience|Salary|
+---------+---+----------+------+
|    Krish| 31|        10| 30000|
|Sudhanshu| 30|         8| 25000|
|    Sunny| 29|         4| 20000|
|     Paul| 24|         3| 20000|
|   Harsha| 21|         1| 15000|
|  Shubham| 23|         2| 18000|
+---------+---+----------+------+
'''

# Show key values
training.printSchema()
'''
root
 |-- Name: string (nullable = true)
 |-- age: integer (nullable = true)
 |-- Experience: integer (nullable = true)
 |-- Salary: integer (nullable = true)
'''

print(f">> Columns: {training.columns}")
# >> Columns: ['Name', 'age', 'Experience', 'Salary']

#############
# ML functions
#############
from pyspark.ml.feature import VectorAssembler

# ["age", "Experience"] --> "features" --> independent features

feature_assembler=VectorAssembler(
    inputCols=["age", "Experience"],
    outputCol="Independent Features"
)

output=feature_assembler.transform(training)
output.show()
'''
+---------+---+----------+------+--------------------+
|     Name|age|Experience|Salary|Independent Features|
+---------+---+----------+------+--------------------+
|    Krish| 31|        10| 30000|         [31.0,10.0]|
|Sudhanshu| 30|         8| 25000|          [30.0,8.0]|
|    Sunny| 29|         4| 20000|          [29.0,4.0]|
|     Paul| 24|         3| 20000|          [24.0,3.0]|
|   Harsha| 21|         1| 15000|          [21.0,1.0]|
|  Shubham| 23|         2| 18000|          [23.0,2.0]|
+---------+---+----------+------+--------------------+
'''

finalized_data = output.select("Independent Features", "Salary")

# Linear Regression
from pyspark.ml.regression import LinearRegression
train_data,test_data = finalized_data.randomSplit([0.7,0.3])
regressor = LinearRegression(featuresCol="Independent Features", labelCol="Salary")
regressor = regressor.fit(train_data)

# Show training
print(f"Coefficients: {regressor.coefficients}")
print(f"Intercept: {regressor.intercept}")
'''
Coefficients: [-64.84641638223238,1584.7554038680123]
Intercept: 15414.10693970376
'''

# Prediction
pred_result = regressor.evaluate(test_data)
pred_result.predictions.show()
'''
+--------------------+------+------------------+
|Independent Features|Salary|        prediction|
+--------------------+------+------------------+
|          [24.0,3.0]| 20000|18612.059158134223|
+--------------------+------+------------------+
'''

# Show prediction metrics
print(f"mean absolute error: {pred_result.meanAbsoluteError}")
print(f"mean squared error: {pred_result.meanSquaredError}")
print(f"root mean squared error: {pred_result.rootMeanSquaredError}")
print(f"r2: {pred_result.r2}")
'''
mean absolute error: 1387.9408418657767
mean squared error: 1926379.780519081
root mean squared error: 1387.9408418657767
r2: -inf
'''
```

### Codificación de Columnas Categóricas
- **Indexación de texto**: `StringIndexer` convierte valores de texto (como nombres de departamentos) en índices numéricos, necesarios para algoritmos de ML.
- **Reversión de índices**: `IndexToString` permite recuperar los valores originales a partir de los índices, útil para interpretar resultados.
- Este proceso permite mantener etiquetas legibles mientras se alimentan representaciones numéricas a los pipelines de ML.

```python
df.show()
'''
+---------+------------+------+
|     Name| Departments|salary|
+---------+------------+------+
|    Krish|Data Science| 10000|
|    Krish|         IOT|  5000|
|   Mahesh|    Big Data|  4000|
|    Krish|    Big Data|  4000|
|   Mahesh|Data Science|  3000|
|Sudhanshu|Data Science| 20000|
|Sudhanshu|         IOT| 10000|
|Sudhanshu|    Big Data|  5000|
|    Sunny|Data Science| 10000|
|    Sunny|    Big Data|  2000|
+---------+------------+------+
'''

# Index columns
from pyspark.ml.feature import IndexToString, StringIndexer
labelIndexer = StringIndexer(inputCols=["Departments"], outputCols=["DepartmentsIndex"])
df_index = labelIndexer.fit(df).transform(df)
df_index.show()
'''
+---------+------------+------+----------------+
|     Name| Departments|salary|DepartmentsIndex|
+---------+------------+------+----------------+
|    Krish|Data Science| 10000|             1.0|
|    Krish|         IOT|  5000|             2.0|
|   Mahesh|    Big Data|  4000|             0.0|
|    Krish|    Big Data|  4000|             0.0|
|   Mahesh|Data Science|  3000|             1.0|
|Sudhanshu|Data Science| 20000|             1.0|
|Sudhanshu|         IOT| 10000|             2.0|
|Sudhanshu|    Big Data|  5000|             0.0|
|    Sunny|Data Science| 10000|             1.0|
|    Sunny|    Big Data|  2000|             0.0|
+---------+------------+------+----------------+
'''

# Convert index to string
indexToString = IndexToString(inputCol="DepartmentsIndex", outputCol="Departments_converted")
df_index = indexToString.transform(df_index)
df_index.show()
'''
+---------+------------+------+----------------+---------------------+
|     Name| Departments|salary|DepartmentsIndex|Departments_converted|
+---------+------------+------+----------------+---------------------+
|    Krish|Data Science| 10000|             1.0|         Data Science|
|    Krish|         IOT|  5000|             2.0|                  IOT|
|   Mahesh|    Big Data|  4000|             0.0|             Big Data|
|    Krish|    Big Data|  4000|             0.0|             Big Data|
|   Mahesh|Data Science|  3000|             1.0|         Data Science|
|Sudhanshu|Data Science| 20000|             1.0|         Data Science|
|Sudhanshu|         IOT| 10000|             2.0|                  IOT|
|Sudhanshu|    Big Data|  5000|             0.0|             Big Data|
|    Sunny|Data Science| 10000|             1.0|         Data Science|
|    Sunny|    Big Data|  2000|             0.0|             Big Data|
+---------+------------+------+----------------+---------------------+
'''
```
