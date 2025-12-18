# motivation.md

## 1. Contexto general
El conjunto de datos empleado recoge información sobre **películas españolas producidas entre 1940 y 2025**, incluyendo variables como el título, año de estreno, género, sinopsis, duración, y otros datos descriptivos.  

A partir del dataset original, se ha generado un **nuevo conjunto de datos procesado** que incorpora información adicional derivada del análisis lingüístico y semántico de las sinopsis. El propósito de este nuevo dataset es estudiar la evolución del cine español desde una perspectiva de **representación de género femenino**.


## 2. Hipótesis de estudio
> **Hipótesis:** *La presencia de personajes principales femeninos en las películas españolas ha aumentado a lo largo de los años.*

El objetivo es comprobar si, con el paso del tiempo, las películas españolas reflejan una **mayor presencia femenina** en sus narrativas, medida a través de menciones, adjetivos y descripciones dentro de las sinopsis. 
Para el estudio se va a suponer que la presencia de un personaje en la sinopsis muestra que forma parte de los protagonistas.

Esta hipótesis se apoya en la idea de que el cine puede actuar como reflejo de los cambios sociales y culturales, por lo que una mayor visibilidad de personajes femeninos en las películas podría estar correlacionada con la evolución de la igualdad de género en la sociedad española.


## 3. Descripción del dataset resultante
El dataset final, denominado **`df_entrega_2.csv`**, combina variables originales del conjunto inicial con **nuevas variables creadas** a partir del análisis textual, semántico y temporal.

### Estructura general del dataset
Cada fila representa **una película española**.  
Las columnas abarcan información **temporal, descriptiva, temática y lingüística**, que permite realizar análisis tanto cuantitativos como cualitativos.

### Variables incluidas
| Categoría | Variable | Tipo de dato | Descripción |
|------------|-----------|---------------|--------------|
| **Identificación temporal** | `year` | numérico | Año de estreno de la película. |
|  | `lustrum` | numérico | Agrupación por intervalos de cinco años (p. ej. 1980, 1985, 1990…). |
|  | `count_by_lustrum` | numérico | Número total de películas estrenadas en el mismo lustro. |
| **Contenido descriptivo** | `genre_simplified` | categórico | Género cinematográfico principal, unificado en categorías comunes (Drama, Comedia, Terror, etc.). |
|  | `duration_minutes` | numérico | Duración estimada de la película en minutos. |
| **Indicadores de representación de género** | `count_female` | numérico | Número de menciones o entidades femeninas detectadas en la sinopsis. |
|  | `count_male` | numérico | Número de menciones o entidades masculinas detectadas. |
|  | `female_presence` | binario (0/1) | Indica si existe o no presencia femenina en la sinopsis. |
|  | `female_ratio` | numérico (0–1) | Proporción de menciones femeninas respecto al total (`female / (female + male)`). |
| **Análisis lingüístico del texto** | `word_plot_count` | numérico | Número total de palabras en la sinopsis. |
|  | `female_adj_percentage` | numérico (%) | Porcentaje de adjetivos femeninos empleados en el texto. |
|  | `media_sentiment` | categórico (POS, NEU, NEG) | Polaridad general de la sinopsis según análisis de sentimiento. |
| **Información contextual adicional** | `topics_lustrum_keywords` | texto | Palabras clave representativas de los temas predominantes por periodo temporal (tópicos por lustro). |


> En conjunto, el dataset contiene variables que describen **qué tipo de película es (género y duración)**, **cuándo se produjo (dimensión temporal)** y **cómo representa a los personajes femeninos (dimensión lingüística y de contenido)**.


## 4. Indicadores analizados
Con el fin de verificar la hipótesis planteada, se calcularon los siguientes indicadores:

| Indicador | Descripción | Propósito |
|------------|--------------|-----------|
| **female_presence** | 1 si la sinopsis incluye alguna referencia femenina, 0 en caso contrario. | Medir la frecuencia de representación femenina. |
| **count_female / count_male** | Recuento de menciones femeninas y masculinas. | Comparar la visibilidad entre géneros. |
| **female_ratio** | Proporción de menciones femeninas respecto al total. | Cuantificar el equilibrio de género. |
| **female_adj_percentage** | Porcentaje de adjetivos femeninos en la sinopsis. | Analizar la carga descriptiva asociada a mujeres. |
| **count_by_lustrum** | Número de películas por lustro. | Contextualizar la evolución temporal del cine español. |


Estos indicadores permiten observar tendencias y posibles cambios estructurales en la representación femenina del cine español a lo largo de ocho décadas.


## 5. Indicadores exploratorios descartados

Durante el proceso de análisis, también se calcularon **indicadores de sentimiento** y **tópicos temáticos** a partir de las sinopsis. Aunque estos no se han utilizado finalmente para la comprobación de la hipótesis, se incluyen aquí por su relevancia metodológica.

### Análisis de sentimiento
Se aplicaron tres modelos distintos:
- **TextBlob**, para obtener una polaridad numérica de la sinopsis.  
- **pysentimiento/robertuito-sentiment-analysis**, un modelo entrenado específicamente para texto en español.  
- **nlptown/bert-base-multilingual-uncased-sentiment**, que clasifica frases en una escala de 1 a 5 estrellas.  

De la combinación de los tres se generó una etiqueta común (`POS`, `NEU`, `NEG`) mediante una media mayoritaria.  
Sin embargo, la gran mayoría de sinopsis resultaron **neutras**, debido a que los resúmenes de las películas suelen tener un tono descriptivo o informativo, sin carga emocional explícita.  
Por este motivo, **el indicador de sentimiento no aportó información relevante** para el análisis de la representación de género y se descartó del estudio principal.

### Análisis de tópicos (BERTopic)
Se entrenó un modelo **BERTopic** sobre las sinopsis agrupadas por lustro, con el fin de identificar los temas predominantes en cada periodo.  
El modelo generó palabras clave representativas (`topics_lustrum_keywords`), pero:
- Los tópicos resultantes fueron **demasiado generales o difusos**, sin conexión clara con la presencia femenina.  
- Muchos textos eran breves, lo que limitaba la capacidad del modelo para identificar temas significativos.  

Por tanto, aunque se incluyen las variables `topics_lustrum_keywords` y `media_sentiment` en el dataset como información complementaria para algún otro enfoque de estudio, no serán empleadas en el desarrollo del projecto.


## 6. Propósito analítico
El objetivo principal del dataset no es solo cuantificar menciones femeninas, sino **evaluar patrones de representación**:
- ¿Aumenta la presencia de mujeres en las narrativas con el paso de los años?  
- ¿Qué géneros cinematográficos muestran mayor o menor representación femenina?  
- ¿Existen diferencias relevantes en el uso del lenguaje (por ejemplo, más adjetivos femeninos en ciertos periodos o géneros)?  

Este conjunto de datos ofrece una base sólida para estudios de tipo **sociolingüístico, histórico o cultural**, además de ser útil para análisis exploratorios, visualizaciones o modelización estadística en temas de género.

## 7.Limitaciones y posibles sesgos
A pesar del valor analítico del dataset, existen limitaciones que deben tenerse en cuenta:
- **Sesgo en las sinopsis:** Las sinopsis no siempre reflejan con precisión el contenido o los personajes principales de la película. 
- **Desigualdad temporal en los datos:** Hay más información disponible sobre películas recientes que sobre las antiguas lo que puede generar una falsa percepción de aumento en la representación femenina.
- **Limitaciones lingüísticas del análisis automático:** Los modelos de procesamiento del lenguaje pueden cometer errores en la desambiguación de nombres o en la identificación del género, especialmente en estos textos breves.

Estas limitaciones no invalidan las conclusiones del estudio, pero tienen que ser contempladas.

## 8. Conclusión
El dataset final permite abordar la hipótesis desde una perspectiva empírica, combinando información cuantitativa (frecuencias, proporciones) con análisis lingüístico.  
Este conjunto de datos sienta las bases para un estudio más profundo sobre la **evolución del papel femenino en el cine español**. 


