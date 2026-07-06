# Reporte de optativo cursado en la Universidad de Texas de Austin
__________________________________________________________________
El presente reporte da cuenta de las actividades realizadas a lo largo del décimo semestre (optativo) de la Licenciatura en Restauración de Bienes Muebles de la presente alumna en las bibliotecas de la Universidad de Texas, Austin. Este semestre se llevó a cabo del 2 de marzo al 2 de junio del 2026. 
De manera general, el semestre tuvo un enfoque explorativo. La alumna tuvo la oportunidad de realizar varias prácticas que le permitieron aproximarse a la disciplina de la gestión de colecciones, la administración digital de archivos, y la preservación digital desde múltiples enfoques bajo la supervisión de bibliotecarios y archivistas. 
Las actividades realizadas se organizaron por medio de rotaciones, es decir, periodos cortos de trabajo en los que se tuvo la oportunidad de trabajar con diversos profesionales de la red de bibliotecas universitarias y realizar distintas labores. Bajo esta lógica, a continuación, cada apartado representa una rotación dentro de este marco temporal. 

## Análisis textual del cedulario W .B. Stepehens-496 perteneciente a la colección Benson Latin American Collection (BLAC) de la Universidad de Texas en Austin

La segunda rotación que se llevó a cabo en este programa consistió en el análisis de la transcripción textual de un cedulario perteneciente a la colección W. B. Stephens, del acervo BLAC (Benson Latin American Collection) de la Universidad de Texas. Para hacer este análisis se utilizaron herramientas como librerías de Python® y plataformas como Transkribus® Gephi®, Palladio®, Dataverse®  y GitHub®.
El cedulario con el número de registro 496 de la colección W. B. Stephens, es una recopilación encuadernada de transcripciones de cédulas enviadas desde la corona española a la Nueva Vizcaya en un marco temporal de 1670-1744. Estas cédulas fueron dictadas por los monarcas Carlos II, Carlos III, Fernando IV, Felipe IV y Felipe V de España a los virreyes de la Nueva Vizcaya. 
A continuación, se describe el proceso llevado a cabo para generar las visualizaciones y metadatos que se encuentran en este dataset.

### a) Transcripción

El primer paso del proceso consistió en revisar la transcripción generada previamente en la plataforma Transkribus® con el modelo de Inteligencia Artificial (IA) llilas_benson_17-18th_italica_cursiva_v4, que tiene un CER (Proporción de error por carácter por sus siglas en inglés ) de 5.3%. Este es un modelo desarrollado por la Universidad de Texas. Fue entrenado específicamente con manuscritos de letra tipo cursiva de los siglos XVII y XVIII, lo que lo hace un modelo de alta confiabilidad para este tipo de tareas. 
Se preproceso el output del modelo manualmente. Esto consistió en eliminar espacios vacíos en la página en donde el modelo identificó texto erróneamente, corregir errores gramaticales y ortográficos que podrían alterar el análisis de manera negativa en las siguientes etapas, y en general, asegurar que la lectura del modelo fuera confiable en cada página. Posteriormente, se exportó a formato texto (.txt). desde la plataforma. 

### b) Extracción de metadatos

A continuación, se utilizó el modelo Claude® AI Sonet 4.6 (Adaptive)  para extraer metadatos de cada cédula. Los campos de metadatos que se utilizaron para este caso fueron el número de registro/id, número de cédula, página, tipo (cédula u orden), lugar de creación, fecha de creación, autor, título, recipiente, una breve descripción, personas mencionadas y finalmente, instituciones y lugares mencionados. Es importante destacar que fue necesario revisar el output del modelo por cada cédula para asegurar coherencia del texto con el output. El prompt utilizado para este resultado fue el siguiente: 
“Proporciona un archivo en formato Table Separated Values (.tsv) que contenga los siguientes campos para este texto: destinatario de la carta; una descripción de 2-3 oraciones en español acerca del contenido de la cédula; una lista de los nombres de las personas a las que se hace referencia, separando múltiples valores con un "|"; nombres de instituciones a los que se hace referencia, separando múltiples valores con un "|"; lista de lugares a los que se hace referencia, separando múltiples valores con un "|". Todas las entidades deberán ser extraídas del archivo de autoridad de nombres de la Biblioteca del Congreso. En caso de no encontrar coincidencias para los nombres de personas, devuélvelas en formato “Apellido, Nombre”. En caso de no encontrar coincidencias para las entidades geográficas o instituciones, devuelve su mención textual. No incluya encabezados”
  
### Named EntIty Recognition con Spacy® y normalización de entidades 

El siguiente paso consistió en generar un script de python® para extraer entidades (sujetos) utilizando la librería Spacy®, es decir, correr un proceso de Named Entity Recognition (Reconocimiento de Entidades Nombradas)  e identificar de su tipo (Misceláneo (MISC), Persona (PER), Organización (ORG) o Localidad(LOC)). Este Script se encuentra disponible en el repositorio de GitHub® del proyecto. El resultado de este proceso fue una hoja de cálculo con dos columnas: entidad y tipo, consistiendo de un total de 611 registros sin duplicados. 
De acuerdo con el IBM, “Una vez recopilado el conjunto de datos, el texto debe limpiarse y formatearse. Es posible que deba eliminar caracteres innecesarios, normalizar el texto o dividir el texto en oraciones o tokens.” (Gomstyn et al., 2026a). Para asegurar la trazabilidad de los datos  y su consultabilidad, una vez se obtuvo este listado, se procedió a la limpieza y normalización de las entidades.  
Este proceso consistió en revisar cada una de las entidades extraídas y compararlas con el registro de Named Authority Files de la Biblioteca del Congreso de los Estados Unidos para identificar su nomenclatura estandarizada. En caso de no existir un registro coincidente de la Biblioteca del Congreso, se mantuvo un formato ‘Apellido, Nombre’.  
A partir de esta información, se generó un diccionario de normalización  que sería posteriormente utilizado para la extracción de coocurrencias con Python®.  La columna izquierda del diccionario corresponde a la mención textual de la entidad en el cedulario, es decir, la extracción pura; mientras que la columna derecha muestra su forma normalizada. Cabe mencionar que la tabla de normalización está disponible en el repositorio de GitHub®. A continuación, se adjunta una captura de pantalla de la hoja de cálculo que resultó de este proceso para mejor comprensión del lector: 
Figura 2: Captura de pantalla del diccionario de normalización del cedulario wbs-496 de la colección W. B. Stephens (BLAC UT) en hojas de cálculo de Google®

### Redes de coocurrencia

Para la generación de las redes de coocurrencia  de las entidades extraídas de todo el cedulario, se generó un script de python® utilizando las librerías de defaultdict, itertools, y Pandas. Este script, al igual que el anterior, se encuentra disponible para consulta en el repositorio de GitHub®. 

Este proceso dio como resultado un archivo comma separated values (.csv) con las columnas de source (origen), target (meta) y weight (peso). La columna source, corresponde al término de origen, es decir, el primer término de la coocurrencia. La columna target enlista el segundo término de la relación. Por último, la columna weight, ilustra la cantidad de ocasiones en que el primer y segundo término se identificaron en la misma cédula. Esta tabla está también disponible en el GitHub® del proyecto.  

### Análisis de dependencia

“Un analizador de dependencias [Dependency Parser en inglés] analiza la estructura gramatical de una oración, estableciendo relaciones entre las palabras "cabeza" y las palabras que modifican esas cabezas” (The Stanford NLP Group, s.f.). Es decir, es un análisis gramatical que describe las relaciones entre las “cabezas” (sujetos) de una oración y su relación con otras palabras o términos en la oración (verbos, adjetivos, adverbios, etc.). Existen varias herramientas que permiten realizar este análisis en grandes cantidades de texto y asignar una etiqueta o tag a cada término, palabra o entidad dependiendo de su función gramatical.  
En el caso del cedulario, había un interés por conocer, específicamente, los adjetivos y verbos relacionados con las entidades (sujetos) referentes a grupos nativos de la región (identificados en la sección ‘c’). De esta manera, se lograría tener una aproximación a la manera en la que la corona española y sus informantes se referían a los pueblos nativos y las acciones que se recuentan en estas cédulas. 
Para este proceso, fue necesario fabricar un tercer script de Python® utilizando las bibliotecas de SpaCy, Counter y Pandas. Este script identificaba aquellos adjetivos y verbos que le corresponden a los distintos términos extraídos referentes a los grupos nativos. El resultado, al igual que en el caso anterior, fue una red de co-ocurrencias. 
Esta tabla contiene una columna con la entidad normalizada, una segunda columna con el verbo o adjetivo que se le atribuía, y una tercera columna con un conteo de las ocasiones en las que esta relación se había identificado a lo largo de todo el cedulario. Es importante mencionar que esta tabla también se encuentra disponible en el repositorio de GitHub® junto con el script utilizado.

### Coocurrencias de entidades geográficas (LOC)

Como último proceso de análisis, se genero una tercera tabla de redes de coocurrencias que exclusivamente contenían las relaciones entre entidades que el sistema identificó como geográficas, habiéndoles asignado el tag de ‘LOC’ (Locación).
De la misma manera que en el paso ‘d’ y ‘e’, se utilizó un script de Python®, pero en este caso, el input fue un diccionario de normalización que únicamente contenía las entidades con el tag de ‘LOC’, es decir, entidades geográficas. El resultado fue el mismo tipo de tabla con los campos de source, target y weight. Esta tabla, al igual que las anteriores está disponible en el repositorio del proyecto en GitHub®. 

### Graficar red de coocurrencia y de dependency parsing en Gephi®

Tras haber obtenido estas tres tablas de coocurrencias (coocurrencias generales, análisis de dependencia de las entidades referentes a grupos indígenas y coocurrencias de entidades ‘LOC’), se procedió a graficar las primeras dos utilizando el sistema Gephi® . Para esto, se importaron las tablas en formato .csv al software y se modificaron los ajustes predeterminados para obtener una visualización con el algoritmo fruchterman-reingold . 
Una vez se obtuvieron las visualizaciones deseadas, se descargaron en formato de plantilla sigma.js. Esta plantilla permite exportar una carpeta completa que incluye los archivos HTML, CSS, JavaScript y los datos del gráfico, lo que permite alojarlos en diversos repositorios para generar un sitio web interactivo de la visualización.
En el caso de las dos visualizaciones generadas a partir del cedulario, se alojaron en el repositorio de GitHub® del proyecto y se encuentran disponibles en los links https://carms-s-a.github.io/analysis_cedulario_full/network/# y https://carms-s-a.github.io/analysis_cedulario_full/network_misc/# A continuación se muestra un screenshot de los sitios. 
Figura 3 (izq.) y 4 (der.): Capturas de pantalla de los sitios web alojados en GitHub® de las visualizaciones de las redes de co-ocurrencia general y dependency parsing generadas con Gephi®
Estos gráficos muestran las relaciones entre entidades (representadas por nodos) y el peso (weight) de esta relación. Además, son gráficos interactivos que permiten al usuario seleccionar un nodo específico para destacar sus relaciones con otras entidades o realizar búsquedas por entidad. 

### Gráfica de coocurrencias de entidades geográficas con Palladio

El siguiente proceso consistió en generar una tabla de coordenadas correspondientes a las entidades referentes a localidades (etiquetadas como ‘LOC’ por SpaCy). Esta tabla contenía una primera columna con la forma normalizada de la entidad y una segunda columna con sus coordenadas.

Esta tabla, junto con la tabla de coocurrencias generada en el paso ‘d)’ (filtrada para solo mostrar las relaciones entre entidades etiquetadas como ‘LOC’) se introdujeron en el sistema de Palladio®  para generar una visualización geográfica de las redes de coocurrencias en el cedulario. A continuación, esta visualización fue también alojada en el mismo repositorio de GitHub® para brindar acceso abiero a ella. La siguiente imagen es una captura de pantalla de la visualización resultante.

Figura 5. Captura de pantalla del sitio web alojado en GitHub® de la visualizacion de las redes de coocurrencia de entidades geográficas generada con Palladio®
Es importante mencionar que además de reflejar las coocurrencias entre entidades, esta visualización muestra el peso de las relaciones mediante el tamaño de cada nodo o entidad.

### Gestión del dataset y almacenamiento en GitHub® repositorio institucional

Como paso final, además del almacenamiento en GitHub®, se almacenó el dataset (La trascripcion del cedulario en formato .txt, las hojas de cálculo generadas en formato .csv y los archivos de las visualizaciones) en el repositorio de DataVerse®  de la Univerisdad de Texas. La estructura de los repositorios y nombres de los archivos se ilustran en el siguiente esquema para facilitar la navegación dentro de los mismos.

__________________________________________________________________
## Bibliografía:

-	Arturo Valdés Oliva - EcuRed. (s.f.). EcuRed. https://www.ecured.cu/Arturo_Valdés_Oliva
-	Gomstyn, A., Jonker, A., & International Business Machines. (2026a). ¿Qué es la normalización de bases de datos? | IBM. IBM. https://www.ibm.com/mx-es/think/topics/database-normalization
-	Gomstyn, A., Jonker, A., & International Business Machines. (2026b). ¿Qué es named entity recognition? | IBM. IBM. https://www.ibm.com/mx-es/think/topics/named-entity-recognition
-	Montalván, G. (1967). Historia del Periodismo de Guatemala. Revista Conservadora, (72), 14–24. https://sajurin.enriquebolanos.org/docs/839.pdf
-	Pasch, Grette. (1881-2016). Arturo Valdés Oliva Papers (Universidad de Texas) Benson Latin American Collection, Austin, Estados Unidos.
-	The Stanford NLP Group. (s.f.). Neural network dependency parser. The Stanford Natural Language Processing Group. https://nlp.stanford.edu/software/nndep.html
-	Universidad de Harvard. (s.f.). About. The Dataverse Proyect. https://dataverse.org/
-	Universidad de Stanford. (s.f.). Palladio-About. Humanities + Design. http://hdlab.stanford.edu/palladio/about/
-	Universidad de Texas, Austin. (2021). Benson Latin American Collection Special Collections Processing Manual.
