# analisis_cedulario
Análisis textual de un cedulario novohispano de los siglo XVII y XVIII perteneciente a la Colección Benson Latin American (BLAC) de la Universidad de Texas en Austin transcrito por medio de Transkribus. Incluye el código utilizado para el análisis, la lista de 'features' obtenidas y el código utilizado para el desarrollo de visualizaciones. 

## Metodología:
1. Selección de texto, investigación histórica
2. Transcripcion del texto con Transkribus y pre-procesamiento manual
3. Modernización y extracción de información para registro .csv inicial con ChatGPT
4. Separación de cédulas y tokenización del texto
5. Extracción de features (NER) con SpaCy en python
6. Extracción de features y normalización (NER) con SpaCy en python y validación de datos en google sheets
7. Selección de features e identificación de objetivios de análisis mediante estudio histórico
8. Elaboracion de diccionario a partir de las features normalizadas de acuerdo al diccionario de autoridades de ña Bibioteca del Congreso
9. Clustering de textos con SpaCy en python
10. Extracción de relaciones con SpaCy en python y exportación de redes (CSV source-target-weight)
11. Creación de nodos y edges y elaboracion de mapas y gráficos con Gephi
12. Validación de corpus
13. Elaboración de visualización pública y repositorio en GitHub
14. Elaboracion de gráficas ilustrativas con matplotlib en python

## Nota:
Este proyecto se encuentra en proceso.
