# Seaborn

**Seaborn** es una biblioteca de visualización de datos en Python que se basa en Matplotlib. Proporciona una interfaz de alto nivel para crear gráficos estadísticos atractivos y fáciles de interpretar. Seaborn se integra estrechamente con las estructuras de datos de pandas, lo que facilita la exploración y comprensión de los datos

## Visualizar gráficas

Para visualizar la gráfica hay que definir los valores para cada eje y luego unir ambas listas en una tabla.

```python
import pandas as pd
import matplotlib.pyplot as plt

import seaborn as sns

df = pd.read_csv('../DATA/heart.csv')

df.head()
```

![alt text](img/02/image.png)

## Graficas de distribución

```python
sns.distplot(df['age'])
```

![alt text](img/02/image-1.png)

## Cambios en la gráfica

```python
plt.figure(figsize=(12, 8))
sns.distplot(df['age'], kde=False, bins=40, color='red')
# kde=False -> Disable kernel density estimate line
# bins=40 -> Change nums of columns
# color='red' -> Select main color

# Límites en el eje x
# plt.xlim(50,60)
```

![alt text](img/02/image-2.png)

## Gráficos de cuentas

Gráficos de cuentas (count plot). Por ejemplo si en una tabla queremos contar número de hombres y mujeres.

```python
sns.countplot(x='sex',data=df)
```

![alt text](img/02/image-3.png)

```python
sns.countplot(x='cp',data=df)
```

![alt text](img/02/image-4.png)

A su misma vez podemos unir las dos gráficas anteriores y contar los datos por sexo.

```python
sns.countplot(x='cp',data=df,hue='sex')
```

![alt text](img/02/image-5.png)

También podemos cambiar la paleta de colores.

<https://matplotlib.org/3.1.0/tutorials/colors/colormaps.html>

```python
sns.countplot(x='cp',data=df,palette='terrain')
```

![alt text](img/02/image-6.png)

## Diagrama de cajas

Diagrama de cajas (Box plot). Este tipo de gráfica nos ayuda a mostrar la distribución de los valores.

![alt text](img/02/image-boxplot.png)

```python
sns.boxplot(x='sex',y='age',data=df)
```

![alt text](img/02/image-7.png)

```python
sns.boxplot(x='target',y='thalach',data=df,hue='sex')
```

![alt text](img/02/image-8.png)

## Diagramas de dispersión

Diagramas de dispersión (Scatter  Plots).

```python
sns.scatterplot(x='chol',y='trestbps',data=df)
```

![alt text](img/02/image-9.png)

```python
sns.scatterplot(x='chol',y='trestbps',data=df,hue='sex',palette='Dark2')
```

![alt text](img/02/image-10.png)

```python
sns.scatterplot(x='chol',y='trestbps',data=df,hue='sex',size='age')
```

![alt text](img/02/image-11.png)

## Diagramas de pares

Diagramas de pares (Pairplots).

```python
iris = pd.read_csv('../DATA/iris.csv')
iris.head()
```

![alt text](img/02/image-12.png)

```python
sns.pairplot(iris)
```

![alt text](img/02/image-13.png)

Nota: Hay que destacar que las gráficas están diplicados en modo espejo con la diagonal.

```python
sns.pairplot(iris, hue="species")
```

![alt text](img/02/image-14.png)
