# analisis_cedulario
Textual analysis of a 17th and 18th century Novohispanic document belonging to the W. B. Stephens-Benson Latin American Collection (BLAC) of the University of Texas at Austin. 
Transcribed with Transkribus. The repository includes the code used for the analysis, the list of 'features' obtained and the code used for the development of visualizations.

I hope this is usefull!
Contact me at: carmensanchez010503@gmail.com

## Trasncription and procesing of the text
1. Transcription to ground truth with Transcribus model llilas_benson_17-18th_italica_cursiva_v4, with a 5.3% CER
2. Modernizing and processing with Claude AI (Sonet 4.6 Adaptive) and Chat-GPT 5.1
3. Metadata extraction with LLM
4. Separation of the cedulas and processing of the text with SpaCy

## Named Entity Recognition
1. Run NER in Python with SpaCy: es_core_news_md
2. Normalization according to the BNE (Biblioteca Nacional de España) name authority files
3. Clean dataset manually referencing primary sources
4. Develop Normalization Dictionary manually
5. Run co-ocurrency pipeline with defaultdict, pandas and itertools
6. Identify coordinates for each location
7. Import to Gephi with Fruchterman and export sigma.js template or to Palladio
8. Host in github.io repository

## Dependency Parsing
1. Run parsing pipeline on MISC type entities (native groups)
2. Clean dataset manually referencing primary sources
3. Export edges in .csv and graph in Palladio
4. Host in github.io repository

## Note:
This is a work in progress. The intentions of it are purely accademic. 
