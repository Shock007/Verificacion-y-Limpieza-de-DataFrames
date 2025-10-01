# Verificación y Limpieza de DataFrames
Este taller está diseñado para practicarse en Google Colab (O si puedes hazlo en Visual Studio Code) utilizando pandas, numpy y matplotlib o seaborn. El objetivo es que los lectores aprendan a detectar, eliminar y rellenar valores faltantes en un DataFrame real.
## Descripción del Dataset
- 5 columnas (ID, Edad, Ingreso_Mensual, Ciudad, Satisfaccion), 70 filas con datos faltantes en
las columnas numéricas. Archivo proporcionado: dataset_ejercicios_limpieza.csv
- **Nombre del DataFrame:** `dataset_ejercicios_limpieza`
- **Columnas relevantes:**
  - 'Edad': Edad registrada en el dataset
  - 'Ingreso_Mensual': Valor monetario guardado en el dataset
  - `Ciudad`: Categoría o ubicación de los registros.
  - `Satisfaccion`: Valor numérico que puede contener valores nulos (NaN).

## Objetivo

Imputar los valores faltantes (`NaN`) en la columna `Satisfaccion`, reemplazándolos con el **promedio de satisfacción por ciudad**. Esto permite preservar las diferencias entre ciudades y evita sesgar los datos al usar un promedio global.

## Lógica del Procedimiento

Ejercicio 1
- Cargar el dataset.
- Usar `df.info()` y `df.isnull().sum()` para detectar valores nulos.
- Graficar un barplot con la cantidad de nulos por columna.

Ejercicio 2
- Crear una copia del DataFrame.
- Eliminar filas con cualquier valor nulo usando dropna().
- Comparar el número de filas antes y después.
- Graficar un histograma de la columna Edad después de la limpieza.

Ejercicio 3
-Rellenar los nulos de Edad con la media.
-Rellenar los nulos de Ingreso_Mensual con la mediana.
-Verificar que ya no queden nulos.
-Graficar un boxplot de Ingreso_Mensual antes y después.

Ejercicio 4
-Agrupar por Ciudad.
-Rellenar los nulos de Satisfaccion con la media por ciudad usando transform y fillna.
-Verificar los nulos restantes y graficar un countplot de Satisfaccion por ciudad.

## Código Utilizado

Primero veamos lo que contiene el dataset.
Usa el siguiente codigo:
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
df = pd.read_csv('/content/dataset_ejercicios_limpieza.csv')
display(df)
```
Con eso importaras las librerias que necesitas, dado caso de que no poseas ninguna ingresa a la terminar de colab e ingresa:
```python
pip install pandas
pip install seaborn
pip install matplotlib
```

Ahora para el ejercicio uno hacemos:
Ejercicio 1 – Inspección de valores faltantes: 1. Cargar el dataset. 2. Usar df.info() y
df.isnull().sum() para detectar valores nulos. 3. Graficar un barplot con la cantidad de nulos por
columna.
```python
#Usamos df.info y df.isnull().sum() para detectar los valores nulos
df.info()

df.isnull().sum()

#Ahora hacemos un grafico de barras para mostrar los valores nulos
sns.barplot(x=df.isnull().sum().index, y=df.isnull().sum())
```

Para el ejercicio 2 hacemos:
Ejercicio 2 – Eliminación de filas incompletas: 
1. Crear una copia del DataFrame.
2. Eliminar filas con cualquier valor nulo usando dropna().
3. Comparar el número de filas antes y después.
4. Graficar un histograma de la columna Edad después de la limpieza.

   
```python
#Copiamos
df_copy = df.copy()
#Eliminamos los valones nulos
df_copy.dropna(inplace=True)
#Comparamos el numero de filas antes y despues
print(f'Filas antes de la limpieza: {len(df)}')
print(f'Filas despues de la limpieza: {len(df_copy)}')

display(df_copy)

#Desplegamos el histograma
sns.histplot(df_copy['Edad'])
```

Para el ejercicio 3 hacemos:
Ejercicio 3 – Relleno con la media o mediana: 
1. Rellenar los nulos de Edad con la media.
2. Rellenar los nulos de Ingreso_Mensual con la mediana.
3. Verificar que ya no queden nulos.
4. Graficar un boxplot de Ingreso_Mensual antes y después.
```python
#Aqui hacemos el calculo de la media para la edad y el calculo de la mediana en Ingreso_Mensual
df_copy['Edad'].fillna(df['Edad'].mean(), inplace=True)
df_copy['Ingreso_Mensual'].fillna(df['Ingreso_Mensual'].median(), inplace=True)
display(df_copy)

#Verificamos que no queden nulos

df_copy.info()

df_copy.isnull().sum()

#Graficamos un boxplot de Ingreso_Mensual antes y después.
sns.boxplot(x=df['Ingreso_Mensual'])
sns.boxplot(x=df_copy['Ingreso_Mensual'])
```

Para el ejercicio 4 hacemos:

Ejercicio 4 – Relleno condicional por grupo: 
1. Agrupar por Ciudad.
2. Rellenar los nulos de Satisfaccion con la media por ciudad usando transform y fillna.
3. Verificar los nulos restantes y graficar un countplot de Satisfaccion por ciudad.
```python
#Seleccionamos la columna de satisfaccion y en ella agrupamos por ciudad, rellenamos los nulos de satisfaccion con la media por ciudad 
df_copy['Satisfaccion'] = df_copy.groupby('Ciudad')['Satisfaccion'].transform(lambda x: x.fillna(x.mean()))
display(df_copy)

#Verificamos que no queden nulos
df_copy.isnull().sum()

#Graficamos un countplot de Satisfaccion por ciudad.
sns.countplot(x=df_copy['Satisfaccion'], hue=df_copy['Ciudad'])
``` 
Ahora compara el numero de filas y columnas antes y despues.
```python
#Comparamos el numero de filas y columnas, antes y despues
print(f'Filas y columnas antes de la limpieza: {df.shape}')
print(f'Filas y columnas despues de la limpieza: {df_copy.shape}')
```
Ahora guarda el dataframe como xlsx o como csv:
```python
# Guardar el DataFrame como archivo Excel o CSV
df_copy.to_excel('df_copy_modificado.xlsx', index=False)
df_copy.to_csv('df_copy_modificado.csv', index=False)
```
Ahora importermos nuestro archivos:
```python
# Importar utilidad de descarga
from google.colab import files
```
Y ahora descargamos:
```python
# Descargar el archivo
files.download('df_copy_modificado.xlsx')
files.download('df_copy_modificado.csv')
```
```
