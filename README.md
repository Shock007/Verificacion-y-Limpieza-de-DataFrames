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

## Código Utilizado

```python
df_copy['Satisfaccion'] = df_copy.groupby('Ciudad')['Satisfaccion'].transform(lambda x: x.fillna(x.mean()))
display(df_copy)
