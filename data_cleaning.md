# data_cleaning.md

## 1. Descripción general
El dataset original contiene información sobre películas españolas producidas entre **1940 y 2025**, con variables relacionadas con el título, año de estreno, género, sinopsis, duración y un identificador único.  
El objetivo del preprocesamiento ha sido **limpiar, normalizar y transformar** los datos para generar un conjunto coherente y utilizable en el análisis de la hipótesis planteada sobre la **presencia femenina en las películas**.


## 2. Carga e inspección inicial
Se ha cargado el dataset `peliculas_1940_2025.csv` en un DataFrame de **pandas**.  
Se han revisado sus dimensiones, tipos de datos, valores únicos y cantidad de valores nulos por columna. Esta inspección ha permitido identificar las variables redundantes y datos faltantes para estudiar cómo tratarlos.


## 3. Tratamiento de valores nulos
- **Columna `release_date`:** contiene una proporción muy alta de valores nulos, siendo redundante. Como el análisis temporal se va a basar en la variable `year`, se ha tomado la decisión de eliminar la columna.  
- **Columna `genre`:** presenta solo 11 valores nulos, por lo que se han eliminado esas filas sin afectar la representatividad del conjunto.  
- **Columna `duration_minutes`:** los valores faltantes se han completado con la **media de duración de películas del mismo lustro**.  Para ello:
  - Se ha creado una nueva columna `lustrum` que agrupa las películas según su año de estreno en intervalos de cinco años (permite observar tendencias más detalladas que una década).
  - A continuación, se han reemplazado los valores nulos por la media del lustro correspondiente.  
  - Finalmente, se han redondeado los valores y convirtido a enteros.


## 4. Detección de duplicados
Se ha verificado que no existen filas duplicadas en el DataFrame mediante `df.duplicated()`.  


## 5. Normalización de la columna 'género' (`genre`)
El dataset presenta una gran dispersión de subgéneros y combinaciones.  
Para homogeneizar la variable se han aplicado las siguientes transformaciones:

- **Normalización textual:** conversión a minúsculas, eliminación de caracteres especiales y espacios sobrantes.  
- **Mapeo inicial:** se ha creado un diccionario `genre_mapping` para agrupar subgéneros en **categorías principales** (Drama, Comedia, Acción, Aventura, Terror, Suspenso, Romance, Documental, Animación, etc.).  
- **Refinamiento manual:** los géneros que quedaron en la categoría “Otros” han sido analizados individualmente, reclasificando mediante búsquedas por palabra clave (por ejemplo, términos como “comedia”, “terror”, “aventuras”, “bélico”, “musical”, “romántica”, “documental”, etc.).  
- El resultado final se ha guardado en una nueva columna `genre_simplified`.


## 6. Cálculo del conteo por lustro
Para conocer la **distribución temporal de las películas** y facilitar la creación de indicadores posteriores, se ha calculado el número de producciones por cada intervalo de cinco años (*lustro*). 
Esto permite generar la variable `count_by_lustrum`, que indica cuántas películas se estrenaron en el mismo periodo temporal, sirviendo como indicador del volumen de producción por etapa.


## 7. Limpieza del texto de las sinopsis (`plot`)
Antes de realizar análisis semánticos y de sentimiento, se prepara el texto con los siguientes pasos:
- Conversión a **minúsculas**.  
- Eliminación de **caracteres no alfabéticos** y espacios repetidos.  
- Eliminación de **stopwords** del idioma español (usando `nltk.stopwords`).  
- Creación de una columna `plot_clean` con el texto limpio.


## 8. Procesamiento del sentimiento
Se han aplicado tres modelos de análisis de sentimiento:
- **TextBlob** (polaridad numérica → POS/NEU/NEG)
- **pysentimiento/robertuito-sentiment-analysis** (POS/NEU/NEG)
- **nlptown/bert-base-multilingual-uncased-sentiment** (clasificación 1–5 estrellas)

Cada resultado se ha transformado a etiquetas comunes (`POS`, `NEU`, `NEG`) y se ha creado la columna `media_sentiment`, con el valor mayoritario entre los tres modelos.

> Finalmente, el análisis ha determiando que la mayoría de sinopsis resultan neutras, por lo que este indicador se descarta en el estudio principal.


## 9. Estudio tópicos
La columna **`topics_lustrum_keywords`** muestra el conjunto de tópicos detectados por BERTopic en las sinopsis de cada periodo (agrupados por lustros).

> Los resultados no han resultado significativos y han sido descartados para el posterior estudio.

## 10. Indicadores de presencia femenina
Se han calculado indicadores específicos de la hipótesis planteada. 
Se ha realizado un análisis lingüístico con **spaCy** y **gender-guesser** para detectar menciones masculinas y femeninas en las sinopsis (tanto instancias de personas con nombres propios como menciones no explícitas):
- **`count_female`** y **`count_male`**: número de menciones o nombres detectados asociados a cada género.  
- **`female_presence`**: variable binaria (1 si aparece alguna mención femenina y 0 en caso contrario).  
- **`female_ratio`**: proporción de menciones femeninas respecto al total (`female / (female + male)`).  
- **`female_adj_percentage`**: porcentaje de adjetivos femeninos detectados en el texto.

Además, se ha generado otra columna que puede ser útil:
- **`word_plot_count`**: número de palabras de la sinopsis.  

## 11. Selección y exportación de variables
Se han elimiando columnas no necesarias para el análisis posterior como `título`, `plot`, `id`, `plot_clean` o `sentiment_bertstars` entre otras.

El DataFrame final, con las variables procesadas y los indicadores creados, ha sido guardado como **`df_entrega_2.csv`**.


