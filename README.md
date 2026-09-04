# 🎵 MLY1101 - EP1 Caso C: Spotify Tracks

## Inteligencia musical y análisis de popularidad de canciones

Proyecto correspondiente a la Evaluación Parcial N°1 de la asignatura **MLY1101 - Machine Learning**.

El proyecto analiza un conjunto de datos de canciones de Spotify con el objetivo de comprender la relación entre sus características musicales y la variable `popularity`, dejando preparada la información para una futura solución de regresión.

> **Importante:** esta etapa del proyecto se concentra en la comprensión del problema, análisis exploratorio (EDA), evaluación de calidad y preparación de los datos. No se entrenan modelos de Machine Learning en esta versión.

---

## 📌 1. Descripción del proyecto

El proyecto busca responder la siguiente problemática:

> **¿Las características musicales y los metadatos disponibles permiten comprender y posteriormente estimar la popularidad (`popularity`) de una canción?**

El análisis busca identificar patrones y relaciones entre las características musicales de las canciones y su nivel de popularidad.

Los resultados pueden servir como apoyo para el análisis de catálogos musicales y como base para una futura etapa de modelamiento predictivo.

Sin embargo, `popularity` no debe interpretarse como una medida directa de calidad artística ni como una relación causal entre las características musicales y el éxito de una canción.

---

## 🎯 2. Objetivos

### Objetivo general

Analizar y preparar el conjunto de datos Spotify Tracks para una futura solución de regresión orientada a estimar la variable `popularity`.

### Objetivos específicos

- Identificar y documentar la fuente de datos utilizada.
- Evaluar la calidad general del dataset.
- Analizar valores faltantes, duplicados y posibles inconsistencias.
- Explorar las principales características estadísticas de las variables.
- Identificar relaciones entre las características musicales y `popularity`.
- Analizar diferencias de popularidad según género musical y contenido explícito.
- Detectar valores potencialmente inválidos y valores atípicos.
- Preparar los datos para una futura etapa de modelamiento.
- Prevenir posibles problemas de data leakage.
- Identificar riesgos relacionados con sesgos, ética y privacidad.

---

## 📊 3. Dataset

El proyecto utiliza el archivo:

```text
data/dataset.csv
```

El conjunto contiene aproximadamente:

- **114.000 registros**
- **21 variables**

Entre las variables disponibles se encuentran:

### Identificación y metadatos

- `track_id`
- `artists`
- `album_name`
- `track_name`
- `track_genre`

### Variable objetivo

- `popularity`

### Características musicales

- `duration_ms`
- `danceability`
- `energy`
- `key`
- `loudness`
- `mode`
- `speechiness`
- `acousticness`
- `instrumentalness`
- `liveness`
- `valence`
- `tempo`
- `time_signature`

### Característica adicional

- `explicit`

La fuente principal utilizada corresponde al dataset entregado para el **Caso C: Spotify Tracks**. El material disponible no permite determinar con certeza el método original de captura del dataset.

---

## 🛠️ 4. Tecnologías y herramientas

El proyecto fue desarrollado utilizando:

- **Python**
- **Pandas** - manipulación y análisis de datos.
- **NumPy** - cálculos numéricos.
- **Matplotlib** - visualización.
- **SciPy** - análisis estadístico y pruebas de asociación.
- **Jupyter Notebook**
- **Google Colab** como alternativa de ejecución.

---

## 📁 5. Estructura del proyecto

Se recomienda mantener la siguiente estructura:

```text
proyecto-spotify/
│
├── data/
│   └── dataset.csv
│
├── notebooks/
│   └── EP1_Caso_C_Spotify_Informe.ipynb
│
├── README.md
│
└── ...
```

El notebook utiliza rutas relativas para facilitar su ejecución en distintos entornos.

---

## ▶️ 6. Ejecución del proyecto

### Opción A: Jupyter Notebook

1. Descargar o clonar el proyecto.
2. Verificar que `dataset.csv` se encuentre dentro de la carpeta:

```text
data/dataset.csv
```

3. Abrir el notebook:

```text
notebooks/EP1_Caso_C_Spotify_Informe.ipynb
```

4. Reiniciar el kernel.
5. Ejecutar todas las celdas desde el inicio.

---

### Opción B: Google Colab

El notebook puede ejecutarse en Google Colab.

Si no encuentra automáticamente:

```text
data/dataset.csv
```

se proporciona una alternativa para cargar el archivo manualmente.

---

## 🔎 7. Análisis exploratorio de datos (EDA)

El notebook realiza un análisis exploratorio que incluye:

### Calidad de datos

- Dimensiones del dataset.
- Tipos de datos.
- Cardinalidad.
- Valores faltantes.
- Porcentaje de valores faltantes.
- Duplicados exactos.
- Identificadores repetidos.

### Estadística descriptiva

Se calculan:

- conteo;
- media;
- desviación estándar;
- mínimo;
- cuartiles;
- máximo;
- percentiles adicionales para `popularity`.

### Distribución de `popularity`

Se analiza la variable objetivo mediante:

- histograma;
- boxplot;
- estadística descriptiva;
- análisis de asimetría.

### Variables musicales

Se estudian variables como:

- duración;
- danceability;
- energy;
- loudness;
- speechiness;
- acousticness;
- instrumentalness;
- liveness;
- valence;
- tempo;
- key;
- mode;
- time_signature.

### Relaciones con `popularity`

Se calculan correlaciones:

- Pearson;
- Spearman.

Las correlaciones se utilizan como medidas descriptivas de asociación y no como evidencia de causalidad.

### Análisis categórico

Se analizan diferencias descriptivas de `popularity` según:

- género musical (`track_genre`);
- contenido explícito (`explicit`).

---

## 🧹 8. Calidad y preparación de datos

El proceso de preparación se realiza de manera conservadora para evitar eliminar información sin justificación.

Se crea una copia del dataset original:

```python
df_clean = df.copy()
```

### Valores faltantes

Los valores faltantes son identificados dinámicamente.

Los faltantes presentes en variables de metadatos textuales pueden ser reemplazados por:

```text
Desconocido
```

Esto permite conservar los registros para el análisis.

### Duplicados

Se diferencian dos situaciones:

1. **Duplicados exactos:** filas completamente idénticas.
2. **`track_id` repetidos:** identificadores que aparecen en más de un registro.

Un `track_id` repetido no implica necesariamente que las filas sean idénticas.

Por este motivo, los registros con identificadores repetidos **no se eliminan automáticamente**.

### Índice artificial

La columna `Unnamed: 0` solo se elimina cuando se comprueba que corresponde exactamente a un índice secuencial artificial.

### Tipos de datos

Se realizan conversiones justificadas, entre ellas:

```text
explicit → boolean
track_genre → category
```

---

## 📈 9. Detección de valores inválidos y outliers

El notebook valida rangos esperados para las principales variables numéricas.

Entre ellas:

- `popularity`
- variables normalizadas entre 0 y 1;
- `key`;
- `mode`;
- `time_signature`;
- `tempo`;
- `duration_ms`.

También se utiliza el método del **rango intercuartílico (IQR)** para identificar posibles valores atípicos.

Se analizan principalmente:

- `duration_ms`
- `loudness`
- `tempo`
- `speechiness`
- `instrumentalness`
- `liveness`
- `popularity`

### Criterio utilizado

Se considera potencialmente atípico un valor que se encuentre fuera de:

```text
Límite inferior = Q1 - 1.5 × IQR
Límite superior = Q3 + 1.5 × IQR
```

Los valores detectados **no se eliminan automáticamente**.

Un outlier puede representar una observación válida del fenómeno estudiado y requiere contexto antes de decidir su tratamiento.

---

## 🔐 10. Prevención de Data Leakage

Para una futura etapa de Machine Learning se considera:

### Variable objetivo

```text
popularity
```

### Variables que no deben utilizarse como predictores base

```text
track_id
```

debido a que corresponde a un identificador.

Además, variables como:

```text
artists
album_name
track_name
```

se consideran metadatos de alta cardinalidad y se mantienen como información descriptiva.

En la futura etapa de modelamiento, la separación entre entrenamiento y prueba deberá realizarse antes de ajustar:

- imputadores;
- escaladores;
- codificadores;
- otros transformadores.

Esto permite evitar que información del conjunto de prueba influya en el entrenamiento.

---

## 🔄 11. Metodología CRISP-DM

El proyecto sigue las etapas de CRISP-DM:

### 1. Business Understanding
Definición del problema de negocio y objetivos.

### 2. Data Understanding
Exploración inicial, estructura y calidad de los datos.

### 3. Data Preparation
Limpieza, transformación y preparación preliminar.

### 4. Modeling
**Pendiente para la siguiente etapa.**

### 5. Evaluation
**Pendiente para la siguiente etapa.**

### 6. Deployment
Fuera del alcance de esta evaluación.

---

## 📏 12. KPIs

Para la futura etapa de modelamiento se utilizarán:

### MAE
Mean Absolute Error.

Permite medir el error absoluto promedio de las predicciones.

### RMSE
Root Mean Squared Error.

Penaliza en mayor medida los errores grandes.

### R²
Coeficiente de determinación.

Permite evaluar qué proporción de la variabilidad de la variable objetivo es explicada por el modelo.

> Estos indicadores todavía no presentan resultados porque esta versión del proyecto no entrena modelos de Machine Learning.

También se utilizan indicadores de calidad del dataset, como:

- porcentaje de valores faltantes;
- duplicados exactos;
- registros con identificadores repetidos;
- cantidad de valores atípicos detectados.

---

## ⚖️ 13. Sesgos, ética y privacidad

El análisis considera posibles fuentes de sesgo, entre ellas:

- distribución desigual de géneros musicales;
- exposición previa de determinadas canciones;
- promoción;
- contexto temporal;
- factores históricos no observados.

La popularidad de una canción puede estar influenciada por factores que no están presentes en el dataset.

Por esta razón:

- una correlación no debe interpretarse como causalidad;
- `popularity` no debe considerarse equivalente a calidad artística;
- no se deben realizar decisiones automatizadas de exclusión basadas únicamente en estas variables.

Respecto a privacidad, el dataset contiene principalmente información asociada a canciones y artistas y no datos personales directos de usuarios.

Se recomienda:

- minimizar el uso de información innecesaria;
- mantener control sobre el acceso a los datos;
- evitar vincular estos datos con perfiles personales sin una justificación adecuada.

---

## 🚫 14. Alcance actual

Esta entrega se concentra exclusivamente en:

```text
Comprensión del problema
        ↓
Comprensión de los datos
        ↓
Análisis exploratorio
        ↓
Evaluación de calidad
        ↓
Preparación de datos
```

No se incluyen en esta versión:

- entrenamiento de modelos;
- comparación de algoritmos;
- optimización de hiperparámetros;
- evaluación de modelos;
- deployment.

Estas actividades corresponden a etapas posteriores del proyecto.

---

## ✅ 15. Checklist de cumplimiento

El notebook contempla los principales indicadores de la pauta:

| Indicador | Estado |
|---|---|
| IE1 - Fuentes de datos y herramientas | ✅ Implementado |
| IE2 - Manipulación y preparación de datos | ✅ Implementado |
| IE3 - Análisis exploratorio y calidad | ✅ Implementado |
| IE4 - Sesgos, ética y privacidad | ✅ Implementado |

### IE1
Se documenta la fuente de datos, herramientas utilizadas, dimensiones del dataset y limitaciones respecto a su captura.

### IE2
Se implementa manipulación y preparación mediante Python, incluyendo copia del dataset, revisión de índices, valores faltantes, tipos, validaciones y selección preliminar de variables.

### IE3
Se incluye análisis exploratorio, estadística descriptiva, calidad de datos, duplicados, valores inválidos, outliers, gráficos, correlaciones y análisis categórico.

### IE4
Se consideran posibles sesgos, aspectos éticos, privacidad, limitaciones y uso responsable de los resultados.

---

## 📌 16. Consideraciones finales

El análisis permite obtener una visión general de las características del dataset Spotify Tracks y de sus relaciones descriptivas con `popularity`.

Los resultados deben interpretarse dentro de las limitaciones del conjunto de datos.

En particular:

> **Las asociaciones encontradas no demuestran causalidad y la popularidad no representa necesariamente la calidad artística de una canción.**

La siguiente etapa del proyecto deberá utilizar los datos preparados para construir y evaluar modelos de regresión mediante una separación adecuada entre entrenamiento y prueba, utilizando MAE, RMSE y R² como métricas principales.

---

## 👥 17. Equipo

**Asignatura:** MLY1101 - Machine Learning  
**Evaluación:** EP1 - Caso C: Spotify Tracks  
**Institución:** Duoc UC  
**Año:** 2026

### Integrantes

- **Sebastián Aird**
- **Bastián Roman**
- **Vicente Contreras**
- **Ignacio Gómez**

---

## 📚 18. Entregables

El proyecto considera:

```text
├── README.md
├── data/
│   └── dataset.csv
└── notebooks/
    └── EP1_Caso_C_Spotify_Informe.ipynb
```

El notebook constituye el principal documento técnico del análisis y contiene las etapas necesarias para reproducir el trabajo realizado.