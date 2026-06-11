Ejercicio 1 Data Science III - Análisis de sentimiento

# Análisis de Sentimiento sobre Reseñas de Yelp

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

---

## Resultados

| Modelo | Accuracy | Precision | Recall | F1 Score |
|----------|----------:|----------:|----------:|----------:|
| VADER | 0.8507 | 0.8630 | 0.9680 | 0.9125 |
| TextBlob | 0.8419 | 0.8524 | 0.9716 | 0.9081 |
| Logistic Regression | 0.8929 | 0.8889 | 0.9905 | 0.9370 |

---

## Análisis de Resultados

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

## Conclusiones

La comparación realizada demuestra que los modelos supervisados pueden superar a los enfoques léxicos cuando se dispone de datos etiquetados para entrenamiento.

La Regresión Logística combinada con TF-IDF obtuvo el mejor rendimiento global, alcanzando una Accuracy de 89,29% y un F1-Score de 93,70%.

Los métodos basados en léxicos continúan siendo alternativas útiles por su simplicidad e interpretabilidad, pero presentan limitaciones para capturar patrones complejos del lenguaje presentes en las reseñas.

Finalmente, este trabajo permitió analizar la importancia del preprocesamiento textual, la representación vectorial de documentos y la selección adecuada de métricas de evaluación en escenarios con desbalance de clases.

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
