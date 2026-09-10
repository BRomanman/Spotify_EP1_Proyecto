# Informe Técnico Completo Spotify Tracks EP1

## 1. Descripción del problema de negocio

El Caso C analiza si las características musicales y los metadatos disponibles permiten comprender y posteriormente estimar `popularity` de una canción. La salida puede apoyar análisis de catálogo, pero no representa una funcionalidad real de Spotify ni debe usarse como una medida de calidad artística.

## 2. Objetivos del proyecto

El objetivo general es analizar y preparar los datos para una futura solución de regresión. Los objetivos específicos son identificar fuentes, revisar calidad, explorar patrones, preparar variables sin fuga de información y evaluar sesgos, ética y privacidad.

## 3. KPIs

Para modelamiento se utilizarán MAE, RMSE y R²; no se inventan valores porque todavía no se entrena un modelo. Para calidad se calculan porcentaje de faltantes y duplicados exactos en el notebook.

## 4. Fuentes de datos y herramientas

La fuente principal es `data/dataset.csv` del Caso C Spotify Tracks: 114,000 registros y 21 variables. Incluye identificadores, metadatos, `popularity` y atributos musicales. Se usan Python, Pandas, NumPy, Matplotlib, SciPy y Jupyter Notebook/Google Colab para el análisis. Colab/Jupyter es un entorno de ejecución, no una fuente de datos. El material disponible no permite determinar con certeza el método original de captura del dataset.

Como entorno de trabajo colaborativo se utiliza Google Colab, que permite edición y ejecución simultánea entre los 4 integrantes del equipo sin depender de que cada uno configure un entorno Python idéntico, relevante dado el tiempo acotado de la evaluación (5 horas en sala de proyectos). El dataset y los entregables se comparten mediante Google Drive, asegurando que el equipo trabaje siempre sobre la misma versión de los archivos.

## 5. Importación de librerías

El notebook importa explícitamente NumPy, Pandas, Matplotlib, SciPy e IPython display. Cada biblioteca está justificada por cálculos, manipulación, gráficos o visualización de tablas realizados en el análisis.

## 6. Carga de datos

La carga busca de manera relativa `data/dataset.csv`, `../data/dataset.csv` y `dataset.csv`; en Google Colab ofrece carga manual solo si no encuentra el archivo. No depende de rutas absolutas.

## 7. Inspección inicial

Se presentan dimensiones, muestra inicial, tipos y memoria mediante `df.info()`. El perfil de calidad informa tipo, faltantes, porcentaje de faltantes, cardinalidad y alertas por columna.

## 8. Valores faltantes

El análisis dinámico identifica: artists: 1, album_name: 1, track_name: 1. Los metadatos textuales faltantes se reemplazan por `Desconocido` únicamente para conservar trazabilidad descriptiva; no se usan como predictores base.

## 9. Duplicados

Se detectaron 0 duplicados exactos (0.000%) sobre el dataset original y 24,259 registros adicionales con `track_id` repetido (89,741 `track_id` únicos de 114,000 filas). Ambos conceptos se separan: un identificador repetido no implica necesariamente una fila idéntica ni autoriza eliminación automática.

El resultado de 0 duplicados exactos se explica porque la columna `Unnamed: 0` actúa como índice único por fila. Al eliminarla en la preparación de datos (sección 18), el mismo chequeo (`duplicated()`) sobre `df_clean` pasa de 0 a **450 duplicados exactos** — es decir, 450 filas que sí repiten todo su contenido una vez removido el índice artificial. Este cambio queda documentado dinámicamente en la comparación antes/después (sección 19) y no se elimina automáticamente, siguiendo el mismo criterio conservador aplicado al resto del dataset.

## 10. Tipos de datos y cardinalidad

El notebook reporta cardinalidad y corrige solo tipos pertinentes: `explicit` se representa como booleano nullable y `track_genre` como categoría. Las conversiones no se hacen arbitrariamente.

## 11. Estadística descriptiva

Se muestra `count`, media, desviación estándar, mínimos, cuartiles y máximos de las variables numéricas. Para `popularity` se reportan además percentiles 1, 5, 25, 50, 75, 95 y 99.

## 12. Variable objetivo popularity

`popularity` tiene media 33.24, mediana 35.00, desviación estándar 22.31, mínimo 0 y máximo 100. El notebook incorpora histograma, boxplot y asimetría calculada dinámicamente.

## 13. Análisis de variables musicales

Se revisan duración, danceability, energy, key, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo y time_signature. La descripción estadística y los gráficos permiten evaluar rango, dispersión y concentración antes de un modelo.

## 14. Correlaciones y relaciones con popularity

Se calculan Pearson y Spearman sobre todos los registros, se ordenan por asociación absoluta y se visualizan en un heatmap y en un gráfico de barras horizontales que compara ambos coeficientes lado a lado. La asociación positiva de Pearson más alta es `loudness` (0.050) y la negativa más baja es `instrumentalness` (-0.095). Estas cifras describen asociación, no causalidad. Que Spearman muestre el mismo patrón débil descarta que la debilidad de Pearson se deba solo a no linealidad. También se incluyen gráficos para danceability, energy, loudness, acousticness, instrumentalness, valence, tempo y duration_ms.

## 15. Análisis por género musical

Se calcula conteo, media y mediana de popularity por género. La visualización muestra los extremos (los 5 géneros con menor y los 8 con mayor popularidad promedio, de 114 géneros en total) en vez de una muestra por frecuencia, para que el gráfico refleje directamente la dispersión entre géneros; el mayor promedio observado corresponde a `pop-film` y el menor a `iranian`, con más de 55 puntos de diferencia — condicionado por la cobertura del conjunto y el tamaño de cada grupo.

## 16. Contenido explícito y popularity

Se presenta una tabla de conteo, media, mediana y desviación estándar por `explicit`, junto a un boxplot y un gráfico de barras con las medias de cada grupo (32.9 vs. 36.5). Las diferencias entre grupos se interpretan como descriptivas; no demuestran que el contenido explícito cause popularidad ni justifican exclusiones automáticas. Se calcula además Cramér's V entre `explicit` y `track_genre` (0.402) y se visualiza el top 10 de géneros por % de contenido explícito (encabezado por `comedy`, 65.6%, muy por sobre el 8.55% promedio general), lo que sugiere que la diferencia observada podría estar parcialmente confundida con el género.

## 17. Valores inválidos y valores atípicos

Se validan rangos esperados para popularity, variables normalizadas entre 0 y 1, key, mode, time_signature, tempo y duration_ms. La identificación IQR y los boxplots se ejecutan en el notebook para duration_ms, loudness, tempo, speechiness, instrumentalness, liveness y popularity, junto con un gráfico de barras que resume el % de outliers de las 7 variables en un solo vistazo. Los extremos se reportan y no se eliminan automáticamente: pueden ser observaciones válidas o errores que requieren contexto.

## 18. Preparación y transformación de datos

Se crea `df_clean = df.copy()`. La columna `Unnamed: 0` se elimina solo si coincide exactamente con un índice secuencial artificial. Luego se tratan los faltantes de metadatos y se ajustan los tipos pertinentes, preservando las observaciones para el EDA.

## 19. Dataset preparado para el análisis

El notebook entrega un nuevo perfil del conjunto preparado con tipo, faltantes y valores únicos por columna. La preparación es deliberadamente conservadora: no borra outliers, canciones con ID repetido ni variables potencialmente útiles sin evidencia.

## 20. Selección preliminar de variables y prevención de data leakage

`popularity` es la variable objetivo. `track_id` se excluye por ser identificador; artists, album_name y track_name se reservan como metadatos de alta cardinalidad. Las variables musicales, explicit y track_genre son candidatas. La futura separación train/test debe ocurrir antes de ajustar imputadores, escaladores o codificadores.

## 20.1 Reformulaciones del problema de predicción

Definir `popularity` como target continuo (regresión) es el enfoque principal del proyecto, pero no la única forma razonable de plantearlo. Antes de cerrar la comprensión de datos, se documentan tres reformulaciones adicionales evaluadas como parte del análisis, no como una decisión ya tomada:

**Clasificación binaria (detección de "hits").** Se define `is_hit` como `popularity >= 80`, umbral que corresponde exactamente al percentil 99 del dataset (no es un número redondo elegido por conveniencia). Resulta en una clase positiva minoritaria (1,201 canciones, 1.05%), visualizada con un gráfico de torta que deja ver de inmediato lo extremo del desbalance, por lo que *accuracy* no es una métrica adecuada: se recomienda precision, recall, F1 o AUC-PR, junto con `class_weight` o remuestreo en la etapa de modelamiento.

**Clasificación multiclase (categorías de popularidad).** Se evaluaron cortes fijos ("redondos") versus cortes por terciles de los datos. Se optó por terciles (cortes en 22 y 45, no en 33/66) porque generan 3 categorías balanceadas (~33% cada una), mientras que cortes redondos producen clases muy desiguales dada la asimetría de `popularity`. Si estos cortes se usan para entrenar un modelo, deben recalcularse únicamente con el conjunto de entrenamiento para evitar fuga de información.

**Reestructuración a nivel canción (género multi-hot).** Se evalúa colapsar el dataset de 114,000 filas (canción × género) a 89,741 filas (una por canción única), representando los géneros como columnas binarias — una codificación multi-hot/multi-label, no one-hot clásico, ya que una canción puede pertenecer a más de un género (hasta 9 en este dataset). Antes de reestructurar se valida que las variables de audio son constantes por canción (confirmado) y se detecta que 720 canciones (4.3% de las que tienen múltiples géneros) presentan `popularity` distinto según el género con que fueron indexadas; se resuelve promediando, decisión documentada explícitamente. Si se usa esta versión del dataset para modelar, los targets `is_hit` y `categoria_popularidad` deben recalcularse sobre el `popularity` ya promediado, no reutilizarse desde el dataset en formato largo.

Estas tres reformulaciones no son excluyentes entre sí: quedan documentadas como opciones evaluadas en la etapa de comprensión de datos; la selección final se hará en la etapa de modelamiento según el desempeño empírico frente a los KPIs definidos.

### Riesgo de fuga de datos por la estructura canción × género

Si el modelamiento se realiza sobre el formato largo, un `train_test_split` aleatorio por fila puede dejar la misma canción en train y en test simultáneamente (16,299 canciones aparecen en más de una fila, con variables de audio idénticas entre sus apariciones). El split debe hacerse agrupado por `track_id` (por ejemplo con `GroupShuffleSplit`), o bien utilizarse el dataset a nivel canción de la reestructuración anterior, que evita este riesgo de forma estructural.

### Modelo sugerido para la etapa de modelamiento

Se recomienda un ensamble basado en árboles (Random Forest como línea base, Gradient Boosting — XGBoost o LightGBM — como modelo principal), sustentado en evidencia generada en este mismo análisis: las asociaciones lineales individuales son débiles (máximo |r| = 0.095), lo que sugiere relaciones no lineales o de interacción que los árboles capturan sin especificación manual; `track_genre` es de alta cardinalidad y es la variable más asociada a `popularity`; no se eliminaron outliers deliberadamente, y los árboles son robustos a ellos por construcción; se requiere interpretabilidad (`feature_importances_`, SHAP) para el seguimiento ético de la sección 22; y el desbalance del target "hit" se maneja de forma nativa con `class_weight`/`scale_pos_weight`. Una regresión lineal regularizada (Ridge/Lasso) puede incluirse como línea base adicional de referencia.

## 21. Metodología CRISP DM

Business Understanding, Data Understanding y Data Preparation están cubiertas. Modeling y Evaluation quedan pendientes: se sugiere partir con un ensamble de árboles (ver sección 20.1) y evaluar MAE, RMSE y R² sobre datos de prueba (o F1/AUC-PR si se adopta alguna de las reformulaciones de clasificación). Deployment está fuera del alcance de la evaluación.

## 22. Sesgos ética y privacidad

La distribución desigual entre géneros, la exposición previa, promoción, contexto temporal y factores históricos no observados podrían introducir sesgo. Popularidad no equivale a calidad artística. No deben realizarse decisiones automatizadas de exclusión. El conjunto contiene información de canciones y artistas, no datos personales directos de usuarios; se recomienda minimización, acceso controlado y no vinculación con perfiles personales.

## 23. Conclusiones y próximas etapas

El dataset permite una exploración sólida, pero las variables aisladas no deben interpretarse como explicaciones causales. Además de la regresión sobre `popularity`, quedan documentadas dos reformulaciones de clasificación (hit binario y categorías por terciles) y una reestructuración a nivel canción (sección 20.1), como opciones a evaluar empíricamente. La siguiente fase debe construir un pipeline reproducible, separar train/test de forma agrupada por `track_id` para evitar fuga de datos, comparar modelos de regresión (sugerido: ensambles de árboles) con las métricas definidas y revisar desempeño por género.