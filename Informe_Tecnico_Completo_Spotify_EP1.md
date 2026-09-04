# Informe Técnico Completo Spotify Tracks EP1

## 1. Descripción del problema de negocio

El Caso C analiza si las características musicales y los metadatos disponibles permiten comprender y posteriormente estimar `popularity` de una canción. La salida puede apoyar análisis de catálogo, pero no representa una funcionalidad real de Spotify ni debe usarse como una medida de calidad artística.

## 2. Objetivos del proyecto

El objetivo general es analizar y preparar los datos para una futura solución de regresión. Los objetivos específicos son identificar fuentes, revisar calidad, explorar patrones, preparar variables sin fuga de información y evaluar sesgos, ética y privacidad.

## 3. KPIs

Para modelamiento se utilizarán MAE, RMSE y R²; no se inventan valores porque todavía no se entrena un modelo. Para calidad se calculan porcentaje de faltantes y duplicados exactos en el notebook.

## 4. Fuentes de datos y herramientas

La fuente principal es `data/dataset.csv` del Caso C Spotify Tracks: 114,000 registros y 21 variables. Incluye identificadores, metadatos, `popularity` y atributos musicales. Se usan Python, Pandas, NumPy, Matplotlib, SciPy y Jupyter Notebook/Google Colab. Colab/Jupyter es un entorno de ejecución, no una fuente de datos. El material disponible no permite determinar con certeza el método original de captura del dataset.

## 5. Importación de librerías

El notebook importa explícitamente NumPy, Pandas, Matplotlib, SciPy e IPython display. Cada biblioteca está justificada por cálculos, manipulación, gráficos o visualización de tablas realizados en el análisis.

## 6. Carga de datos

La carga busca de manera relativa `data/dataset.csv`, `../data/dataset.csv` y `dataset.csv`; en Google Colab ofrece carga manual solo si no encuentra el archivo. No depende de rutas absolutas.

## 7. Inspección inicial

Se presentan dimensiones, muestra inicial, tipos y memoria mediante `df.info()`. El perfil de calidad informa tipo, faltantes, porcentaje de faltantes, cardinalidad y alertas por columna.

## 8. Valores faltantes

El análisis dinámico identifica: artists: 1, album_name: 1, track_name: 1. Los metadatos textuales faltantes se reemplazan por `Desconocido` únicamente para conservar trazabilidad descriptiva; no se usan como predictores base.

## 9. Duplicados

Se detectaron 0 duplicados exactos (0.000%) y 24,259 registros adicionales con `track_id` repetido. Ambos conceptos se separan: un identificador repetido no implica necesariamente una fila idéntica ni autoriza eliminación automática.

## 10. Tipos de datos y cardinalidad

El notebook reporta cardinalidad y corrige solo tipos pertinentes: `explicit` se representa como booleano nullable y `track_genre` como categoría. Las conversiones no se hacen arbitrariamente.

## 11. Estadística descriptiva

Se muestra `count`, media, desviación estándar, mínimos, cuartiles y máximos de las variables numéricas. Para `popularity` se reportan además percentiles 1, 5, 25, 50, 75, 95 y 99.

## 12. Variable objetivo popularity

`popularity` tiene media 33.24, mediana 35.00, desviación estándar 22.31, mínimo 0 y máximo 100. El notebook incorpora histograma, boxplot y asimetría calculada dinámicamente.

## 13. Análisis de variables musicales

Se revisan duración, danceability, energy, key, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo y time_signature. La descripción estadística y los gráficos permiten evaluar rango, dispersión y concentración antes de un modelo.

## 14. Correlaciones y relaciones con popularity

Se calculan Pearson y Spearman sobre todos los registros, se ordenan por asociación absoluta y se visualizan en un heatmap. La asociación positiva de Pearson más alta es `loudness` (0.050) y la negativa más baja es `instrumentalness` (-0.095). Estas cifras describen asociación, no causalidad. También se incluyen gráficos para danceability, energy, loudness, acousticness, instrumentalness, valence, tempo y duration_ms.

## 15. Análisis por género musical

Se calcula conteo, media y mediana de popularity por género. La visualización usa solo los 15 géneros más frecuentes para evitar un gráfico ilegible; el mayor promedio observado corresponde a `pop-film`, condicionado por la cobertura del conjunto y el tamaño de cada grupo.

## 16. Contenido explícito y popularity

Se presenta una tabla de conteo, media, mediana y desviación estándar por `explicit`, junto a un boxplot. Las diferencias entre grupos se interpretan como descriptivas; no demuestran que el contenido explícito cause popularidad ni justifican exclusiones automáticas.

## 17. Valores inválidos y valores atípicos

Se validan rangos esperados para popularity, variables normalizadas entre 0 y 1, key, mode, time_signature, tempo y duration_ms. La identificación IQR y los boxplots se ejecutan en el notebook para duration_ms, loudness, tempo, speechiness, instrumentalness, liveness y popularity. Los extremos se reportan y no se eliminan automáticamente: pueden ser observaciones válidas o errores que requieren contexto.

## 18. Preparación y transformación de datos

Se crea `df_clean = df.copy()`. La columna `Unnamed: 0` se elimina solo si coincide exactamente con un índice secuencial artificial. Luego se tratan los faltantes de metadatos y se ajustan los tipos pertinentes, preservando las observaciones para el EDA.

## 19. Dataset preparado para el análisis

El notebook entrega un nuevo perfil del conjunto preparado con tipo, faltantes y valores únicos por columna. La preparación es deliberadamente conservadora: no borra outliers, canciones con ID repetido ni variables potencialmente útiles sin evidencia.

## 20. Selección preliminar de variables y prevención de data leakage

`popularity` es la variable objetivo. `track_id` se excluye por ser identificador; artists, album_name y track_name se reservan como metadatos de alta cardinalidad. Las variables musicales, explicit y track_genre son candidatas. La futura separación train/test debe ocurrir antes de ajustar imputadores, escaladores o codificadores.

## 21. Metodología CRISP DM

Business Understanding, Data Understanding y Data Preparation están cubiertas. Modeling y Evaluation quedan pendientes y deberán evaluar MAE, RMSE y R² sobre datos de prueba. Deployment está fuera del alcance de la evaluación.

## 22. Sesgos ética y privacidad

La distribución desigual entre géneros, la exposición previa, promoción, contexto temporal y factores históricos no observados podrían introducir sesgo. Popularidad no equivale a calidad artística. No deben realizarse decisiones automatizadas de exclusión. El conjunto contiene información de canciones y artistas, no datos personales directos de usuarios; se recomienda minimización, acceso controlado y no vinculación con perfiles personales.

## 23. Conclusiones y próximas etapas

El dataset permite una exploración sólida, pero las variables aisladas no deben interpretarse como explicaciones causales. La siguiente fase debe construir un pipeline reproducible, separar train/test, comparar modelos de regresión con las métricas definidas y revisar desempeño por género.

## 24. Checklist de cumplimiento

| Indicador | Evidencia |
|---|---|
| IE1 | Fuente, herramientas, justificación y limitación de captura documentadas. |
| IE2 | Copia, índice artificial, faltantes, tipos, validaciones y selección preliminar implementadas. |
| IE3 | Perfil, calidad, descriptivos, outliers, gráficos, correlaciones y análisis categórico implementados. |
| IE4 | Sesgos potenciales, ética, privacidad, limitaciones y uso responsable documentados. |
