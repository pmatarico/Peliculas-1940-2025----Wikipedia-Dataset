# Análisis de la Presencia Femenina en el Cine Español (1940-2025)

Este proyecto de **Descubrimiento de Conocimiento en Datos Complejos** analiza la evolución de la representación femenina en el cine español a lo largo de los años 1940-2025. Utilizando técnicas de Procesamiento de Lenguaje Natural (NLP) y análisis de series temporales, se estudia si existe una tendencia cuantitativa al alza en el protagonismo femenino basada en las sinopsis de las películas.

## Descripción General

El objetivo principal es transformar datos no estructurados (sinopsis de películas) en conocimiento estructurado para validar una hipótesis.

El proyecto abarca desde la limpieza y enriquecimiento de datos hasta la modelización de series temporales para predecir tendencias futuras.

### Hipótesis de Estudio
> **"La presencia de personajes principales femeninos ha aumentado en el cine español a lo largo de los años."**
>
> *Se asume que "aumentar" implica una tendencia temporal con pendiente positiva y significancia estadística (p-valor < 0.05).*

## Datos

El dataset original contiene información de películas españolas producidas entre 1940 y 2025.
- **Fuente original**: [Paulamata/Peliculas1940-2025_WikipediaDataset en Hugging Face](https://huggingface.co/datasets/Paulamata/Peliculas1940-2025_WikipediaDataset)
- **Dataset procesado**: `df_entrega_2.csv` (Generado tras la limpieza y extracción de características).

### Variables Clave Generadas
- `female_presence`: Variable binaria (1 si hay mención femenina en sinopsis, 0 si no).
- `female_ratio`: Proporción de menciones femeninas respecto al total.
- `topics_lustrum_keywords`: Tópicos detectados por lustro mediante BERTopic.
- `media_sentiment`: sentimiento medio de las sinópsis.


## Estructura del Proyecto

El repositorio se organiza de la siguiente manera:

```text
├── data/
│   ├── peliculas_1940_2025.csv    # Dataset original (descargar de Hugging Face)
│   └── df_entrega_2.csv           # Dataset procesado final
├── docs/
│   ├── motivation.md              # Contexto y justificación del proyecto
│   └── data_cleaning.md           # Documentación de la limpieza de datos
├── notebooks/
│   ├── notebook_entrega2.ipynb    # Preprocesamiento, limpieza y NLP (BERTopic, Gender detection)
│   └── notebbok_entrega3.ipynb    # ANÁLISIS FINAL: Series temporales y validación de hipótesis
└── README.md                      # Este archivo
```

## Guía de Instalación y Reproducción

Sigue estos pasos para reproducir los resultados en tu entorno local o en Google Colab.

### 1. Prerrequisitos y Librerías

El proyecto utiliza **Python 3.x**. Las principales librerías necesarias se dividen en procesamiento (NLP) y análisis (Series Temporales).
Puedes instalar las dependencias con el siguiente comando:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
pip install gender-guesser unidecode spacy bertopic
pip install hampel pmdarima prophet tensorflow
pip install vaderSentiment
```
*Nota: Para spacy, asegúrate de descargar el modelo en español:*

```bash
python -m spacy download es_core_news_sm
```

### 2. Flujo de Ejecución

#### Paso 1: Preprocesamiento y NLP
Ejecuta el notebook `notebook_entrega2.ipynb`.

* **Entrada:** Dataset crudo de Hugging Face.
* **Procesos:**
    * Limpieza de nulos y normalización de texto.
    * Detección de género en nombres y sinopsis usando `gender-guesser` y `spaCy`.
    * Modelado de tópicos con `BERTopic`.
    * Análisis de sentimiento mediante `TextBlob`, `pysentimiento/robertuito-sentiment-analysis` y `nlptown/bert-base-multilingual-uncased-sentiment`.
    * Creación de indicadores de presencia femenina en las sinopsis.
    
* **Salida:** Genera el archivo `df_entrega_2.csv`.

#### Paso 2: Análisis de Series Temporales
Ejecuta el notebook `notebbok_act.ipynb`.

* **Entrada:** `df_entrega_2.csv`.
* **Procesos:**
    * Agregación de datos por año y lustro.
    * Análisis los indicadores principales de presencia femenina.
    * Análisis de Estacionariedad: Ejecución del test  **Dickey-Fuller**. 
    * Análisis de Regresión Lineal **(OLS)**: Cálculo de la pendiente y el p-valor para validar la hipótesis de crecimiento lineal a largo plazo.
    * Validación de Varianza **(Test ARCH)**: Evaluación de la homocedasticidad mediante el análisis de la variación al cuadrado de la serie.
    * Detección de outliers con el filtro **Hampel**.
    * Modelado predictivo con **ARIMA / SARIMAX**.
    * Modelado con Redes Neuronales **LSTM**.
* **Resultado:** Gráficas de evolución temporal y validación estadística de la hipótesis.

## Tecnologías Utilizadas

* **Procesamiento de Datos:** `pandas`, `numpy`
* **NLP:** `spaCy`, `BERTopic`, `gender-guesser`
* **Series Temporales:** `pmdarima` (AutoARIMA), `statsmodels`, `prophet`, `hampel`
* **Deep Learning:** `tensorflow` (Keras LSTM)
* **Visualización:** `matplotlib`, `seaborn`

## Autores y Referencias

Este proyecto utiliza datos públicos alojados en Hugging Face.

* **Link al dataset:** [Hugging Face - Peliculas1940-2025](https://huggingface.co/datasets/Paulamata/Peliculas1940-2025_WikipediaDataset)
