Ejercicio 1 Data Science III - Análisis de sentimiento

# Análisis de Sentimiento sobre Reseñas de Yelp

## Descripción

Este proyecto tiene como objetivo comparar dos enfoques de análisis de sentimiento aplicados a reseñas de Yelp:

- Método basado en léxicos utilizando **VADER**.
- Modelo preentrenado utilizando **TextBlob**.

El objetivo es clasificar reseñas como positivas o negativas y comparar el desempeño de ambos métodos mediante métricas de evaluación.

---

## Dataset

Se utilizó el dataset **Yelp Reviews Dataset** disponible en Kaggle:

https://www.kaggle.com/datasets/vivekhn/yelp-reviews

### Características del dataset

- Cantidad original de registros: **10.000 reseñas**
- Columnas: 10 variables descriptivas de usuarios, negocios y reseñas.
- Columna utilizada para el análisis: **text**

### Construcción de la variable objetivo

Las reseñas fueron clasificadas de la siguiente manera:

| Estrellas | Sentimiento |
|------------|------------|
| 1 - 2 | Negativo (0) |
| 4 - 5 | Positivo (1) |
| 3 | Eliminadas |

Las reseñas con 3 estrellas fueron excluidas por representar opiniones neutrales o ambiguas.

### Dataset final

- Positivas: 6.863
- Negativas: 1.676
- Total: 8.539 reseñas

---

## Tecnologías Utilizadas

- Python
- Pandas
- NumPy
- NLTK
- VADER Sentiment Analyzer
- TextBlob
- Scikit-Learn
- Matplotlib
- Seaborn
- Google Colab

---

## Metodología

### 1. Carga y exploración de datos

Se cargó el dataset de Yelp y se analizaron las variables disponibles para identificar la información relevante para el análisis de sentimiento.

### 2. Preparación de los datos

Se eliminaron las reseñas neutrales (3 estrellas) y se creó una variable binaria denominada `sentiment`.

### 3. División de datos

Se dividió el dataset en:

- 80% entrenamiento
- 20% validación

Utilizando:

```python
train_test_split(test_size=0.20, random_state=42)
```

### 4. Métodos evaluados

#### VADER (Enfoque Basado en Léxicos)

VADER (Valence Aware Dictionary and sEntiment Reasoner) utiliza un diccionario de palabras con polaridad y reglas lingüísticas para calcular el sentimiento de un texto.

#### TextBlob (Modelo Preentrenado)

TextBlob utiliza técnicas de Procesamiento de Lenguaje Natural (NLP) para calcular la polaridad de cada reseña y determinar su clasificación.

### 5. Evaluación

Se utilizaron las siguientes métricas:

- Accuracy
- Precision
- Recall
- F1 Score

---

## Resultados

| Modelo | Accuracy | Precision | Recall | F1 Score |
|----------|----------:|----------:|----------:|----------:|
| VADER | 0.8507 | 0.8630 | 0.9680 | 0.9125 |
| TextBlob | 0.8419 | 0.8524 | 0.9716 | 0.9081 |

---

## Análisis de Resultados

Los resultados muestran que ambos enfoques obtuvieron un desempeño satisfactorio en la clasificación de reseñas.

### VADER

- Mayor Accuracy.
- Mayor Precision.
- Mejor F1 Score.
- Menor cantidad de falsos positivos.

### TextBlob

- Mayor Recall.
- Detecta una mayor cantidad de reseñas positivas reales.
- Presenta una ligera disminución en Precision y Accuracy.

### Comparación General

Aunque las diferencias son pequeñas, VADER logró el mejor equilibrio entre Precision y Recall, obteniendo el mejor desempeño global sobre el conjunto de datos analizado.

---

## Conclusiones

En este trabajo se compararon dos enfoques de análisis de sentimiento utilizando reseñas de Yelp.

Los resultados obtenidos muestran que ambos métodos alcanzaron niveles elevados de desempeño, superando el 84% de Accuracy y el 90% de F1 Score.

El modelo basado en léxicos VADER obtuvo los mejores resultados generales, alcanzando:

- Accuracy: 85.07%
- Precision: 86.30%
- Recall: 96.80%
- F1 Score: 91.25%

Por otro lado, TextBlob obtuvo el mejor Recall (97.16%), demostrando una gran capacidad para identificar reseñas positivas.

Finalmente, este trabajo permitió comprender la importancia de utilizar múltiples métricas de evaluación para analizar correctamente el comportamiento de los modelos de análisis de sentimiento.

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
