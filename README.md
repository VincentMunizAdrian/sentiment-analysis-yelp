Ejercicio 1 Data Science III - Análisis de sentimiento

# Análisis de Sentimiento sobre Reseñas de Yelp

## Introduccón

El análisis de sentimiento es una de las aplicaciones más utilizadas dentro del Procesamiento de Lenguaje Natural (NLP). Su objetivo consiste en identificar automáticamente la polaridad de un texto, clasificándolo generalmente como positivo, negativo o neutral.

En este trabajo se comparan diferentes enfoques para el análisis de sentimiento sobre reseñas de Yelp, incluyendo métodos basados en léxicos y modelos supervisados de Machine Learning. Además de evaluar el rendimiento predictivo de cada alternativa, se analizan aspectos relacionados con el desbalance de clases, la selección de métricas y las consideraciones necesarias para un posible despliegue en entornos industriales.

## Descripción del Proyecto

El objetivo de este proyecto es comparar distintos enfoques de análisis de sentimiento aplicados a reseñas de Yelp, evaluando tanto métodos basados en léxicos como modelos supervisados de Machine Learning.

Se analizaron tres enfoques diferentes:

- VADER (enfoque basado en léxicos)
- TextBlob (análisis de polaridad basado en reglas y léxicos)
- Regresión Logística utilizando representación TF-IDF (modelo supervisado)

La comparación se realizó utilizando métricas de clasificación estándar y analizando el impacto del desbalance de clases presente en el conjunto de datos.

---

## Dataset

Se utilizó el dataset Yelp Reviews Dataset disponible en Kaggle.

### Características iniciales

- 10.000 reseñas
- 10 variables descriptivas
- Columna principal utilizada: `text`
- Columna de puntuación: `stars`

### Construcción de la variable objetivo

Se generó una variable binaria de sentimiento utilizando la puntuación de estrellas:

| Estrellas | Sentimiento |
|------------|------------|
| 1 - 2 | Negativo (0) |
| 4 - 5 | Positivo (1) |
| 3 | Eliminadas |

Las reseñas con 3 estrellas fueron excluidas por representar opiniones neutrales o ambiguas.

### Dataset Final

| Clase | Cantidad |
|---------|---------:|
| Positivo | 6863 |
| Negativo | 1676 |
| Total | 8539 |

---

## Análisis del Desbalance de Clases

Luego de la preparación de los datos se observó una distribución desbalanceada:

- Positivas: 80,4%
- Negativas: 19,6%

Este desbalance puede generar interpretaciones erróneas si únicamente se utiliza Accuracy como métrica de evaluación.

Por ejemplo, un clasificador que predijera siempre la clase positiva obtendría aproximadamente un 80% de Accuracy sin ser realmente útil para detectar opiniones negativas.

Por este motivo se incorporaron métricas adicionales:

- Precision
- Recall
- F1-Score

Particularmente, el F1-Score resulta relevante porque combina Precision y Recall en una única medida y permite una evaluación más robusta en datasets desbalanceados.

---

## Preprocesamiento de Texto

Antes de entrenar los modelos se aplicó una etapa de limpieza textual.

Las transformaciones realizadas fueron:

- Conversión a minúsculas
- Eliminación de URLs
- Eliminación de caracteres especiales
- Eliminación de números
- Eliminación de espacios redundantes
- Eliminación de stopwords

### Ejemplo

**Texto original**

> The food was AMAZING!!! I'll definitely come back.

**Texto procesado**

> food amazing definitely come back

Esta etapa permitió reducir ruido y mejorar la representación textual utilizada por el modelo supervisado.

---

## Metodología

### División de Datos

El conjunto de datos fue dividido en:

- 80% entrenamiento
- 20% validación

Utilizando estratificación para conservar la distribución de clases.

### Métodos Evaluados

#### VADER

VADER (Valence Aware Dictionary and sEntiment Reasoner) es un método basado en léxicos que utiliza diccionarios de polaridad y reglas lingüísticas para calcular el sentimiento de un texto.

#### TextBlob

TextBlob utiliza análisis de polaridad basado en recursos léxicos y reglas gramaticales para estimar el sentimiento de un texto.

Aunque frecuentemente se utiliza para tareas de análisis de sentimiento, no constituye un modelo supervisado entrenado específicamente sobre este conjunto de datos.

#### Regresión Logística + TF-IDF

Como enfoque supervisado se utilizó una Regresión Logística entrenada sobre representaciones TF-IDF de las reseñas.

**Proceso aplicado:**

1. Vectorización mediante TF-IDF.
2. Entrenamiento de Regresión Logística.
3. Predicción sobre el conjunto de validación.
4. Evaluación mediante métricas de clasificación.

---

## Métricas de Evaluación

Las métricas utilizadas fueron:

- Accuracy
- Precision
- Recall
- F1 Score

### Definiciones

- **Accuracy:** porcentaje total de predicciones correctas.
- **Precision:** proporción de predicciones positivas correctas.
- **Recall:** capacidad para detectar correctamente los casos positivos.
- **F1 Score:** media armónica entre Precision y Recall.

### Selección de Métricas según el Contexto de Negocio

En este proyecto se utilizó F1-Score como métrica principal debido al desbalance existente entre las clases positivas y negativas.

Sin embargo, la métrica más importante depende del problema de negocio que se desea resolver.

Si el costo asociado a los Falsos Positivos fuera considerablemente superior al de los Falsos Negativos, la métrica prioritaria sería Precision.

Por ejemplo, en un sistema de detección de fraude o moderación automática de contenido, clasificar incorrectamente un elemento legítimo como fraudulento podría generar pérdidas económicas, bloqueos injustificados o afectar negativamente la experiencia de los usuarios.

En estos escenarios se busca minimizar la cantidad de Falsos Positivos, incluso si esto implica perder algunos casos positivos reales.

Por el contrario, en aplicaciones donde resulta crítico detectar la mayor cantidad posible de casos positivos, como sistemas de diagnóstico médico o detección temprana de incidentes, la métrica más relevante suele ser Recall.

Por lo tanto, la selección de métricas debe estar alineada con los objetivos y riesgos específicos del contexto de aplicación.

---

## Resultados

| Modelo | Accuracy | Precision | Recall | F1 Score |
|----------|----------:|----------:|----------:|----------:|
| VADER | 0.8507 | 0.8630 | 0.9680 | 0.9125 |
| TextBlob | 0.8419 | 0.8524 | 0.9716 | 0.9081 |
| Logistic Regression | 0.8929 | 0.8889 | 0.9905 | 0.9370 |

### Resumen de Resultados

Los resultados muestran que la Regresión Logística obtuvo el mejor desempeño global en todas las métricas evaluadas.

Respecto a los enfoques léxicos, VADER presentó un rendimiento ligeramente superior a TextBlob en Accuracy, Precision y F1-Score.

La mejora observada en la Regresión Logística puede atribuirse a su capacidad para aprender patrones directamente a partir de los datos de entrenamiento, mientras que los enfoques léxicos dependen principalmente de diccionarios y reglas predefinidas.

---

## Interpretación de la Matriz de Confusión

La matriz de confusión permite analizar en detalle el comportamiento del modelo más allá de las métricas agregadas.

- **True Positives (TP):** reseñas positivas clasificadas correctamente como positivas.
- **True Negatives (TN):** reseñas negativas clasificadas correctamente como negativas.
- **False Positives (FP):** reseñas negativas clasificadas erróneamente como positivas.
- **False Negatives (FN):** reseñas positivas clasificadas erróneamente como negativas.

A partir de estos valores es posible calcular manualmente Precision, Recall y F1-Score, verificando los resultados obtenidos mediante Scikit-Learn.

La baja cantidad de errores observada en la matriz de confusión explica los elevados valores de Accuracy, Precision, Recall y F1-Score obtenidos por el modelo de Regresión Logística.

---

## Análisis Comparativo de Resultados

Los resultados muestran que el modelo supervisado obtuvo el mejor desempeño general.

### VADER

**Ventajas**

- Fácil implementación.
- No requiere entrenamiento.
- Alta interpretabilidad.

**Limitaciones**

- Dependencia de diccionarios predefinidos.
- Dificultad para capturar patrones específicos del dominio.

### TextBlob

**Ventajas**

- Implementación sencilla.
- Buena capacidad para estimar polaridad.

**Limitaciones**

- Basado en reglas y léxicos.
- Menor capacidad de adaptación al dominio específico.

### Logistic Regression + TF-IDF

**Ventajas**

- Aprende patrones directamente a partir de los datos.
- Mejor Accuracy.
- Mejor Precision.
- Mejor Recall.
- Mejor F1 Score.

**Limitaciones**

- Requiere entrenamiento.
- Necesita preprocesamiento y representación vectorial.

---

## Consideraciones Industriales

### Despliegue de los Modelos

Desde una perspectiva industrial, la elección del modelo no depende únicamente de su rendimiento predictivo, sino también de factores operativos como costo computacional, mantenimiento y escalabilidad.

VADER y TextBlob presentan la ventaja de no requerir entrenamiento previo. Esto permite desplegarlos rápidamente en sistemas productivos y obtener predicciones inmediatas.

Por otro lado, la Regresión Logística requiere una fase de entrenamiento y la conservación tanto del modelo entrenado como del vectorizador TF-IDF utilizado durante el aprendizaje. Aunque esto aumenta la complejidad del sistema, también permite obtener mejores resultados predictivos.

### Coste Computacional

| Modelo | Entrenamiento | Inferencia |
|----------|----------|----------|
| VADER | No requiere | Muy bajo |
| TextBlob | No requiere | Muy bajo |
| Logistic Regression | Moderado | Bajo |

Los enfoques léxicos son extremadamente eficientes desde el punto de vista computacional, mientras que los modelos supervisados requieren una inversión inicial para el entrenamiento.

Sin embargo, una vez entrenada, la Regresión Logística puede realizar predicciones de forma rápida incluso sobre grandes volúmenes de datos.

### Escalabilidad

En entornos empresariales es común procesar miles o incluso millones de reseñas diariamente.

Los métodos léxicos presentan una gran capacidad de escalado debido a su simplicidad y bajo consumo de recursos.

La Regresión Logística también puede escalar adecuadamente utilizando infraestructuras distribuidas y pipelines automatizados de procesamiento de texto.

Por esta razón, los modelos supervisados suelen ser una alternativa viable cuando se busca un mejor rendimiento predictivo sin incrementar excesivamente el costo operacional.

### Limitaciones de los Modelos

#### Sarcasmo

Una limitación importante es la dificultad para interpretar expresiones sarcásticas.

Ejemplo:

> "Great, another hour waiting for my food."

Aunque la palabra "Great" posee una connotación positiva, el significado real de la frase es negativo.

#### Lenguaje Multilingüe

Los modelos fueron evaluados utilizando reseñas en inglés.

Su desempeño podría verse afectado al procesar textos en otros idiomas sin una adaptación previa del pipeline de procesamiento.

#### Opiniones Mixtas

Los modelos utilizados generan una única clasificación global para cada reseña.

Por ejemplo:

> "The food was amazing but the service was terrible."

En este caso existen opiniones positivas y negativas sobre distintos aspectos del servicio, pero el modelo produce una única predicción general.

Esta limitación puede abordarse mediante técnicas de análisis de sentimiento por aspectos (Aspect-Based Sentiment Analysis).

---

## Conclusiones

La comparación realizada demuestra que los modelos supervisados pueden superar a los enfoques léxicos cuando se dispone de datos etiquetados para entrenamiento.

La Regresión Logística combinada con TF-IDF obtuvo el mejor rendimiento global, alcanzando una Accuracy de 89,29% y un F1-Score de 93,70%.

Los métodos basados en léxicos continúan siendo alternativas útiles por su simplicidad e interpretabilidad, pero presentan limitaciones para capturar patrones complejos del lenguaje presentes en las reseñas.

Finalmente, este trabajo permitió analizar la importancia del preprocesamiento textual, la representación vectorial de documentos y la selección adecuada de métricas de evaluación en escenarios con desbalance de clases.

Asimismo, el trabajo permitió comprobar que la elección de métricas no debe realizarse únicamente desde una perspectiva estadística, sino también considerando el contexto de negocio y el costo asociado a los diferentes tipos de error. Este aspecto resulta especialmente relevante en aplicaciones reales donde las decisiones tomadas por los modelos pueden tener impacto operativo, económico o incluso legal.

---

## Trabajo Futuro

Como posibles líneas de mejora para este proyecto se proponen:

- Evaluar modelos supervisados adicionales como Random Forest y Support Vector Machines.
- Incorporar modelos basados en Transformers como BERT.
- Implementar técnicas de análisis de sentimiento por aspectos.
- Desarrollar soporte para múltiples idiomas.
- Analizar estrategias avanzadas para el tratamiento del desbalance de clases.
- Comparar distintas técnicas de representación textual como Word2Vec y embeddings contextuales.

---

## Tecnologías Utilizadas

- Python
- Pandas
- NumPy
- NLTK
- VADER
- TextBlob
- Scikit-Learn
- TF-IDF
- Logistic Regression
- Matplotlib
- Seaborn
- Google Colab

---

## Estructura del Proyecto

```text
sentiment-analysis-yelp/
│
├── README.md
├── yelp.csv
└── yelp_sentiment_analysis.ipynb
```

---

Proyecto desarrollado para la práctica de Análisis de Sentimiento utilizando técnicas de Procesamiento de Lenguaje Natural (NLP).
