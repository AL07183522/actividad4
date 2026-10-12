# UNIVERSIDAD TECMILENIO
## Master Inteligencia Artificial / Gestión de Proyectos de IA

**Materia:** Gestión de Proyectos de Inteligencia Artificial  
**Actividad:** 4  
**Alumno(a):** Manuel de Jesus Ramiro Velázquez Tapia  
**Matrícula:** AL07183522  
**Docente:** Luis Ariel Vazquez Piña  
**Dataset:** `SetFit/tweet_sentiment_extraction` (Muestra de 100 registros del split `train`)  

---

## Tabla de contenidos
1. [Descripción del Proyecto y Objetivos](#descripción-del-proyecto-y-objetivos)
2. [Estructura del Repositorio](#estructura-del-repositorio)
3. [Entorno de Ejecución y Dependencias](#entorno-de-ejecución-y-dependencias)
4. [Dataset: SetFit/tweet_sentiment_extraction](#dataset-setfittweet_sentiment_extraction)
5. [Modelos Evaluados](#modelos-evaluados)
6. [Resultados Cuantitativos y Benchmark](#resultados-cuantitativos-y-benchmark)
7. [Reporte Académico](#reporte-académico)
8. [Referencias Bibliográficas](#referencias-bibliográficas)

---

## Descripción  y objetivos
En esta actividad 4 abordaremos el **ciclo de prototipado rápido de inteligencia artificial** con modelos de lenguaje preentrenados disponibles en la plataforma **Hugging Face** para realizar **análisis de sentimientos** en tweets.

Como objetivo principal tenemos el evaluar cuantitativamente la precisión, F1-Score y latencia de inferencia de tres arquitecturas preentrenadas distintas sobre una muestra representativa de **100 registros extraídos del conjunto de entrenamiento (`train`)** del dataset **`SetFit/tweet_sentiment_extraction`**.

---

## Estructura del repositorio
```text
.
├── Actividad_4.ipynb   # cuaderno funcional de Google Colab
├── train.csv           # dataset
└── README.md           #dDocumentación técnica e informe
```

---

## Entorno de ejecución y dependencias

### Lenguaje y Entorno Cómputo
* **Lenguaje:** Python `3.10+ / 3.12`
* **Entorno de Ejecución:** Google Colab con aceleración hardware GPU NVIDIA Tesla T4 (15 GB VRAM)
* **Versión de PyTorch:** `2.1.0+cu121`
* **CUDA Version:** `12.1`

### Librerías utilizadas y propósito
* `transformers` (v4.38+): Carga de pipelines de inferencia y tokenizadores de Hugging Face.
* `datasets` (v2.18+): Acceso automatizado a la Hugging Face Hub API para la descarga del dataset.
* `torch` (v2.1+): Framework de aprendizaje profundo para aceleración por GPU.
* `pandas` (v2.0+): Manipulación y estructuración de datos tabulares.
* `scikit-learn` (v1.3+): Cálculo riguroso de métricas de clasificación (Accuracy, Precision, Recall, F1-Score).
* `seaborn` & `matplotlib`: Generación de gráficos estadísticos comparativos.
* `huggingface_hub`: Autenticación y gestión de modelos en el Hub de Hugging Face.

---

## Dataset: SetFit/tweet_sentiment_extraction

* **Origen:** Dataset de tweets alojado en Hugging Face Datasets.
* **Muestra Seleccionada:** 100 registros extraídos secuencialmente de la partición de entrenamiento (`split="train[:100]"`).
* **Distribución de Clases:**
  * `neutral`: 43 muestras (43.0%)
  * `negative`: 33 muestras (33.0%)
  * `positive`: 24 muestras (24.0%)
* **Estructura de Campos:** `textID` (Identificador único), `text` (Contenido del tweet), `label` (Índice numérico), `label_text` (Categoría de sentimiento).

---

## Modelos evaluados
1. **RoBERTa (`pysentimiento/robertuito-sentiment-analysis`)**: Modelo RoBERTa-base optimizado específicamente para análisis de sentimientos y tareas de NLP social en español.
2. **DistilBERT Multilingüe (`distilbert-base-multilingual-cased`)**: Versión destilada de mBERT (6 capas, 134M parámetros), diseñada para baja latencia en entornos multilingües.
3. **mBERT Multilingüe (`bert-base-multilingual-cased`)**: Modelo BERT base multilingüe preentrenado en 104 idiomas (12 capas, 177M parámetros).

---

## Resultados cuantitativos y benchmark

| Modelo | Arquitectura | Parámetros | Accuracy | F1-Score (Weighted) | Latencia Promedio (ms) |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **RoBERTa (robertuito)** | RoBERTa-base (Español) | 110M | **72.00%** | **0.7196** | 12.62 ms |
| **DistilBERT Multilingüe** | DistilBERT-cased | 134M | 33.00% | 0.1638 | **7.44 ms** |
| **mBERT Multilingüe** | BERT-base-multilingual | 177M | 33.00% | 0.1638 | 9.33 ms |

### Conclusiones del benchmark
* **RoBERTa (robertuito)** sobresalio en la comprensión de la sintaxis informal de tweets, alcanzando un **72% de exactitud**.
* **DistilBERT** ofreció la menor latencia (7.44 ms/muestra), pero requiere un proceso de *fine-tuning* previo para mapear sus clases numéricas a la taxonomía específica del dataset.
* **Recomendación de Negocio:** Se recomienda el despliegue de **RoBERTa (robertuito)**, ya que la diferencia de latencia (+5.18 ms) es insignificante para el usuario final frente a la ganancia masiva en precisión predictiva (+39%).

---

## Reporte Académico

### Declaración del uso de herramientas de inteligencia artificial
* **Herramientas:** VSCode, Gemini Notebook, Hugging Face Transformers, Google Colab GPU T4.
* **Propósito:** Codificacion, obtencion del dataset, generacion del md, ayuda en codificacion

### Análisis de Trade-offs y decisión de Ajuste
Al evaluar los 3 modelos se demostro que usar un modelo  con la especialización en el dominio del idioma y tipo de texto (tweets) prevalece de forma determinante sobre la capacidad generalista multilingüe. A pesar de que DistilBERT y mBERT cuentan con mayor cantidad de parámetros, su esquema de inferencia zero-shot sin desambiguación de etiquetas resultó en un desempeño cercano al azar (33%).

En conclusion **RoBERTa (robertuito)** se consolida como la eleccion en caso de llevar el prototipo a producción, sin necesidad de inversión inicial en entrenamiento.

---

## Referencias Bibliográficas
* SetFit. (2022). *tweet_sentiment_extraction dataset*. Hugging Face Datasets Repository. https://huggingface.co/datasets/SetFit/tweet_sentiment_extraction

* Pérez, J. M., Rajngewerc, M., Giudici, J. C., Furman, D. A., Luque, F., Alemany, L. A., & Martínez, M. V. (2021). pysentimiento: A Python toolkit for opinion mining and social NLP tasks. arXiv. https://doi.org/10.48550/arXiv.2106.09462

* Sanh, V., Debut, L., Chaumond, J., & Wolf, T. (2019). DistilBERT, a distilled version of BERT: Smaller, faster, cheaper and lighter. arXiv preprint arXiv:1910.01108. https://doi.org/10.48550/arXiv.1910.01108

* Devlin, J., Chang, M.-W., Lee, K., & Toutanova, K. (2018). BERT: Pre-training of deep bidirectional transformers for language understanding. arXiv preprint arXiv:1810.04805. https://doi.org/10.48550/arXiv.1810.04805
