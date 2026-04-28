# 💧 Water Quality Analysis in India — WQI & Deep Learning
### Large-Scale Data Processing · Parcial 2
**Pontificia Universidad Javeriana**

---

## 📌 Overview

This project performs a **large-scale analysis of water quality across Indian river monitoring stations**, using Apache Spark for distributed data processing and a Keras/TensorFlow deep neural network to **predict the Water Quality Index (WQI)**.

The dataset was sourced from India's **Central Pollution Control Board (CPCB)** National Water Quality Monitoring Programme and contains physicochemical and biological parameters measured at stations distributed across the country's states.

---

## 🗂️ Project Structure

```
Parcial 2/
│
├── Clean_ML_Water.ipynb                  # Main analysis and ML notebook
├── waterquality.csv                      # Raw dataset (CPCB water monitoring data)
├── Resultados_Parcial2_CalidadAgua.pdf   # Full analysis report (PDF)
├── Indian_States/                        # Shapefile for geospatial visualization
│   ├── Indian_States.shp                 # State boundary polygons
│   ├── Indian_States.dbf                 # Attribute table
│   ├── Indian_States.shx                 # Shape index
│   └── Indian_States.prj                 # Coordinate reference system (CRS)
└── README.md
```

---

## 📊 Dataset Description

**File:** `waterquality.csv` — Records from river monitoring stations across India.

| Column | Description |
|---|---|
| `STATION CODE` | Unique identifier for each monitoring station |
| `LOCATIONS` | Name and description of the sampling point |
| `STATE` | Indian state where the station is located |
| `TEMP` | Water temperature (°C) |
| `DO` | Dissolved Oxygen (mg/L) |
| `pH` | Acidity/alkalinity level (0–14 scale) |
| `CONDUCTIVITY` | Electrical conductivity (µmhos/cm) |
| `BOD` | Biochemical Oxygen Demand (mg/L) — organic pollution indicator |
| `NITRATE_N_NITRITE_N` | Nitrate + Nitrite Nitrogen concentration (mg/L) |
| `FECAL_COLIFORM` | Fecal coliform bacteria count (MPN/100ml) |
| `TOTAL_COLIFORM` | Total coliform bacteria count (MPN/100ml) |

---

## 🔬 Methodology

### 1. Data Ingestion & Cleaning (Apache Spark)
- Data loaded into a Spark DataFrame and registered as a SQL view.
- Type casting from string to float using `lambda` functions on RDDs.
- Detection and handling of null/invalid values.
- Descriptive statistics: mean, std deviation, min, max per parameter.

### 2. Exploratory Data Analysis (EDA)
Time-series visualizations for each parameter pair:
- **DO vs. pH** — pH remains stable (neutral to slightly alkaline); DO shows high variability indicating organic contamination episodes.
- **BOD vs. Nitrates/Nitrites** — Both show spikes, pointing to periodic organic and chemical contamination.
- **Conductivity vs. Fecal Coliform** — Extreme fecal coliform peaks indicate significant punctual contamination events.

### 3. WQI Feature Engineering
Quality ranges were assigned to each parameter following the **Sutadian et al. (2016)** bibliographic reference, scoring each station on a 0–100 scale:

| Score | Classification |
|---|---|
| 100 | Fresh / Drinkable water |
| 80 | Moderate quality |
| 60 | Hard water |
| 40 | Very hard water |
| 0 | Unacceptable |

Parameters evaluated: `qrPH`, `qrDO`, `qrCOND`, `qrBOD`, `qrNN`, `qrFecal` → aggregated into a single **WQI** column.

### 4. State-Level WQI Visualization
A horizontal bar chart displays the **WQI per Indian state**, revealing strong regional disparities. Most states score above 25, meaning their water does **not** qualify as fresh/drinkable under this criterion.

### 5. Deep Learning Model — WQI Prediction
A feedforward **neural network** was built with Keras/TensorFlow to predict the WQI from the 6 quality-range features:

| Parameter | Value |
|---|---|
| Input features | 6 (`qrPH`, `qrDO`, `qrCOND`, `qrBOD`, `qrNN`, `qrFecal`) |
| Hidden layers | 3 × Dense (350 neurons, ReLU) |
| Output layer | 1 × Dense (linear) |
| Optimizer | Adam (`lr=0.001`) |
| Loss function | Mean Squared Error (MSE) |
| Epochs | 200 |
| Batch size | 81 |
| Train/Test split | 80% / 20% |

The model shows rapid loss convergence in the first epochs, stabilizing near zero, confirming effective learning of the WQI classification patterns.

---

## 🧾 Key Conclusions

1. **Data Preparation:** Type conversion, null value treatment, and descriptive statistics provided a solid understanding of the dataset before modeling.
2. **Parameter Behavior:** pH was generally stable; DO, BOD, and fecal coliform showed high variability, indicating heterogeneous contamination across stations.
3. **WQI Distribution:** Most Indian river stations do not meet fresh/drinkable water standards (WQI < 25), with notable regional differences.
4. **Model Performance:** The neural network captured the general trend of the WQI effectively, converging quickly and showing consistent predictions aligned with the training data patterns.

---

## 🛠️ Technologies & Libraries

| Tool | Purpose |
|---|---|
| Python 3.x | Primary programming language |
| Apache Spark (PySpark) | Distributed data processing and SQL queries |
| Pandas | Data conversion for ML pipeline |
| Matplotlib | Statistical and time-series visualizations |
| GeoPandas | Geospatial mapping with Indian state shapefiles |
| Scikit-Learn | Train/test data splitting |
| Keras + TensorFlow | Deep neural network construction and training |
| Jupyter Notebook | Interactive analysis environment |

---

## 🚀 How to Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/jorgegz10/Procesamiento-de-datos-a-gran-escala.git
   cd "Procesamiento-de-datos-a-gran-escala/Parcial 2"
   ```

2. **Install dependencies:**
   ```bash
   pip install pyspark pandas numpy matplotlib seaborn geopandas scikit-learn keras tensorflow jupyter
   ```

3. **Launch the notebook:**
   ```bash
   jupyter notebook Clean_ML_Water.ipynb
   ```

> ⚠️ A working Apache Spark installation is required. Make sure `JAVA_HOME` is set correctly in your environment.

---

## 📚 References

1. Sutadian, A. D., et al. (2016). *Development of a water quality index for rivers in West Java Province, Indonesia*. IntechOpen, Chapter 69568.
2. Central Pollution Control Board India — National Water Quality Monitoring Programme (RiverIndia Dataset).
3. Apache Spark MLlib Documentation. https://spark.apache.org/docs/latest/ml-guide.html
4. Chollet, F. (2021). *Deep Learning with Python*, 2nd Edition. Manning Publications.
5. Corredor, J. (2026). *Material de clase: Procesamiento de Alto Volumen de Datos*. Pontificia Universidad Javeriana, Bogotá.

---

## 👤 Author

**Jorge Esteban Gómez Zuluaga**
Pontificia Universidad Javeriana

## 👨‍🏫 Professor

**PhD. Jhon Jairo Corredor Franco**
Large-Scale Data Processing — 2026

---
---

# 💧 Análisis de Calidad del Agua en India — WQI y Deep Learning
### Procesamiento de Datos a Gran Escala · Parcial 2
**Pontificia Universidad Javeriana**

---

## 📌 Descripción General

Este proyecto realiza un **análisis a gran escala de la calidad del agua en estaciones de monitoreo fluvial de India**, usando Apache Spark para el procesamiento distribuido de datos y una red neuronal profunda con Keras/TensorFlow para **predecir el Índice de Calidad del Agua (WQI)**.

El dataset proviene del **Central Pollution Control Board (CPCB)** de India, dentro del Programa Nacional de Monitoreo de Calidad del Agua, y contiene parámetros fisicoquímicos y biológicos medidos en estaciones distribuidas por los estados del país.

---

## 🗂️ Estructura del Proyecto

```
Parcial 2/
│
├── Clean_ML_Water.ipynb                  # Cuaderno principal de análisis y ML
├── waterquality.csv                      # Dataset crudo (datos de monitoreo CPCB)
├── Resultados_Parcial2_CalidadAgua.pdf   # Informe completo del análisis (PDF)
├── Indian_States/                        # Shapefile para visualización geoespacial
│   ├── Indian_States.shp                 # Polígonos de límites estatales
│   ├── Indian_States.dbf                 # Tabla de atributos
│   ├── Indian_States.shx                 # Índice de formas
│   └── Indian_States.prj                 # Sistema de referencia de coordenadas (CRS)
└── README.md
```

---

## 📊 Descripción del Dataset

**Archivo:** `waterquality.csv` — Registros de estaciones de monitoreo fluvial en toda India.

| Columna | Descripción |
|---|---|
| `STATION CODE` | Identificador único de cada estación de monitoreo |
| `LOCATIONS` | Nombre y descripción del punto de muestreo |
| `STATE` | Estado de India donde se ubica la estación |
| `TEMP` | Temperatura del agua (°C) |
| `DO` | Oxígeno Disuelto (mg/L) |
| `pH` | Nivel de acidez/alcalinidad (escala 0–14) |
| `CONDUCTIVITY` | Conductividad eléctrica (µmhos/cm) |
| `BOD` | Demanda Bioquímica de Oxígeno (mg/L) — indicador de contaminación orgánica |
| `NITRATE_N_NITRITE_N` | Concentración de Nitratos + Nitritos en Nitrógeno (mg/L) |
| `FECAL_COLIFORM` | Recuento de bacterias coliformes fecales (NMP/100ml) |
| `TOTAL_COLIFORM` | Recuento total de bacterias coliformes (NMP/100ml) |

---

## 🔬 Metodología

### 1. Ingesta y Limpieza de Datos (Apache Spark)
- Datos cargados en un DataFrame de Spark y registrados como vista SQL.
- Conversión de tipos de dato (string → float) usando funciones `lambda` sobre RDDs.
- Detección y tratamiento de valores nulos o inválidos.
- Estadísticas descriptivas: media, desviación estándar, mínimo y máximo por parámetro.

### 2. Análisis Exploratorio de Datos (EDA)
Visualizaciones de series temporales por par de parámetros:
- **DO vs. pH** — El pH se mantiene estable (neutro a ligeramente alcalino); el OD muestra alta variabilidad, indicando episodios de contaminación orgánica.
- **BOD vs. Nitratos/Nitritos** — Ambos presentan picos, señalando contaminación orgánica y química periódica.
- **Conductividad vs. Coliformes Fecales** — Picos extremos de coliformes fecales evidencian eventos de contaminación puntual significativa.

### 3. Ingeniería de Características para el WQI
Se asignaron rangos de calidad a cada parámetro siguiendo la referencia bibliográfica de **Sutadian et al. (2016)**, puntuando cada estación en una escala de 0–100:

| Puntuación | Clasificación |
|---|---|
| 100 | Agua Dulce / Potable |
| 80 | Calidad Moderada |
| 60 | Agua Dura |
| 40 | Agua Muy Dura |
| 0 | Inaceptable |

Parámetros evaluados: `qrPH`, `qrDO`, `qrCOND`, `qrBOD`, `qrNN`, `qrFecal` → agregados en una columna única **WQI**.

### 4. Visualización del WQI por Estado
Un histograma horizontal muestra el **WQI por estado indio**, revelando grandes disparidades regionales. La mayoría de los estados superan el valor de 25, lo que significa que su agua **no** califica como potable bajo este criterio.

### 5. Modelo de Deep Learning — Predicción del WQI
Se construyó una **red neuronal feedforward** con Keras/TensorFlow para predecir el WQI a partir de las 6 características de rango de calidad:

| Parámetro | Valor |
|---|---|
| Variables de entrada | 6 (`qrPH`, `qrDO`, `qrCOND`, `qrBOD`, `qrNN`, `qrFecal`) |
| Capas ocultas | 3 × Dense (350 neuronas, ReLU) |
| Capa de salida | 1 × Dense (lineal) |
| Optimizador | Adam (`lr=0.001`) |
| Función de pérdida | Error Cuadrático Medio (MSE) |
| Épocas | 200 |
| Tamaño de lote | 81 |
| División Entrenamiento/Prueba | 80% / 20% |

El modelo muestra una convergencia rápida de la pérdida en las primeras épocas, estabilizándose cerca de cero, confirmando un aprendizaje efectivo de los patrones de clasificación del WQI.

---

## 🧾 Conclusiones Clave

1. **Preparación de Datos:** La conversión de tipos, el tratamiento de valores nulos y las estadísticas descriptivas proporcionaron una comprensión sólida del dataset antes del modelado.
2. **Comportamiento de los Parámetros:** El pH fue generalmente estable; el OD, la BOD y los coliformes fecales mostraron alta variabilidad, indicando contaminación heterogénea entre estaciones.
3. **Distribución del WQI:** La mayoría de las estaciones fluviales en India no cumplen con los estándares de agua potable (WQI < 25), con notables diferencias regionales.
4. **Desempeño del Modelo:** La red neuronal capturó la tendencia general del WQI de forma efectiva, convergiendo rápidamente y mostrando predicciones consistentes alineadas con los patrones de los datos de entrenamiento.

---

## 🛠️ Tecnologías y Librerías

| Herramienta | Propósito |
|---|---|
| Python 3.x | Lenguaje de programación principal |
| Apache Spark (PySpark) | Procesamiento distribuido de datos y consultas SQL |
| Pandas | Conversión de datos para el pipeline de ML |
| Matplotlib | Visualizaciones estadísticas y de series temporales |
| GeoPandas | Mapeo geoespacial con shapefiles de los estados de India |
| Scikit-Learn | División de datos en entrenamiento y prueba |
| Keras + TensorFlow | Construcción y entrenamiento de la red neuronal profunda |
| Jupyter Notebook | Entorno de análisis interactivo |

---

## 🚀 Cómo Ejecutar

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/jorgegz10/Procesamiento-de-datos-a-gran-escala.git
   cd "Procesamiento-de-datos-a-gran-escala/Parcial 2"
   ```

2. **Instalar dependencias:**
   ```bash
   pip install pyspark pandas numpy matplotlib seaborn geopandas scikit-learn keras tensorflow jupyter
   ```

3. **Lanzar el cuaderno:**
   ```bash
   jupyter notebook Clean_ML_Water.ipynb
   ```

> ⚠️ Se requiere una instalación funcional de Apache Spark. Asegúrate de tener `JAVA_HOME` correctamente configurado en tu entorno.

---

## 📚 Referencias

1. Sutadian, A. D., et al. (2016). *Development of a water quality index for rivers in West Java Province, Indonesia*. IntechOpen, Chapter 69568.
2. Central Pollution Control Board India — National Water Quality Monitoring Programme (RiverIndia Dataset).
3. Apache Spark MLlib Documentation. https://spark.apache.org/docs/latest/ml-guide.html
4. Chollet, F. (2021). *Deep Learning with Python*, 2nd Edition. Manning Publications.
5. Corredor, J. (2026). *Material de clase: Procesamiento de Alto Volumen de Datos*. Pontificia Universidad Javeriana, Bogotá.

---

## 👤 Autor

**Jorge Esteban Gómez Zuluaga**
Pontificia Universidad Javeriana

## 👨‍🏫 Profesor

**PhD. Jhon Jairo Corredor Franco**
Procesamiento de Datos a Gran Escala — 2026
