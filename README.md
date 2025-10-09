# Clasificación de Alzheimer a partir de RMNs con Redes Neuronales Convolucionales

![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)![TensorFlow](https://img.shields.io/badge/TensorFlow-2.12%2B-orange.svg)![Scikit-learn](https://img.shields.io/badge/scikit--learn-1.2%2B-blueviolet.svg)![License](https://img.shields.io/badge/License-MIT-green.svg)

Este proyecto implementa un modelo de Deep Learning para clasificar imágenes de resonancia magnética (RMN) cerebral en cuatro estadios de la enfermedad de Alzheimer. Se sigue la metodología **CRISP-DM** y se utiliza una **Red Neuronal Convolucional (CNN)** construida con TensorFlow y Keras para lograr una alta precisión en la clasificación.

El modelo final alcanza una **precisión del 94.8%** en el conjunto de prueba, demostrando una excepcional capacidad para identificar las características visuales asociadas a cada etapa de la enfermedad.

---

## 📋 Tabla de Contenidos

*   [1. Descripción del Problema](#1-descripción-del-problema)
*   [2. Dataset](#2-dataset)
*   [3. Metodología CRISP-DM](#3-metodología-crisp-dm)
*   [4. Estructura del Proyecto](#4-estructura-del-proyecto)
*   [5. Instalación](#5-instalación)
*   [6. Uso](#6-uso)
*   [7. Arquitectura del Modelo](#7-arquitectura-del-modelo)
*   [8. Resultados y Evaluación](#8-resultados-y-evaluación)
*   [9. Conclusiones](#9-conclusiones)
*   [10. Trabajo Futuro](#10-trabajo-futuro)

---

### 1. Descripción del Problema

El objetivo principal de este proyecto es desarrollar un sistema de clasificación automática que pueda asistir en el diagnóstico temprano y la estadificación de la enfermedad de Alzheimer. El modelo debe ser capaz de analizar una RMN cerebral y clasificarla en una de las siguientes cuatro categorías:

*   **Sin Demencia (Non Demented)**
*   **Demencia Muy Leve (Very Mild Demented)**
*   **Demencia Leve (Mild Demented)**
*   **Demencia Moderada (Moderate Demented)**

Un desafío clave del proyecto es el **severo desbalanceo de clases** en el dataset, lo que requiere estrategias específicas para asegurar que el modelo sea sensible a las clases minoritarias, especialmente a las más severas.

### 2. Dataset

Se utilizó el dataset **"Alzheimer MRI Disease Classification Dataset"** disponible en Kaggle, almacenado en formato Parquet.

➡️ **Fuente:** [https://www.kaggle.com/datasets/borhanitrash/alzheimer-mri-disease-classification-dataset](https://www.kaggle.com/datasets/borhanitrash/alzheimer-mri-disease-classification-dataset)

El dataset contiene 6,400 imágenes de RMN en escala de grises, pre-divididas en conjuntos de entrenamiento (5,120) y prueba (1,280).

### 3. Metodología CRISP-DM

El proyecto está estructurado siguiendo las fases de la metodología CRISP-DM para asegurar un desarrollo riguroso y ordenado.

*   **Fase 1: Comprensión del Negocio:** Definición de objetivos y criterios de éxito.
*   **Fase 2: Comprensión de los Datos:** Análisis Exploratorio de Datos (AED) para identificar características clave, distribución de clases y el desbalanceo.
*   **Fase 3: Preparación de Datos:** Preprocesamiento de imágenes (redimensionamiento, normalización), codificación de etiquetas y cálculo de pesos de clase.
*   **Fase 4: Modelado:** Diseño, construcción y compilación de la arquitectura de la Red Neuronal Convolucional (CNN).
*   **Fase 5: Evaluación:** Entrenamiento del modelo y evaluación exhaustiva utilizando métricas clave.
*   **Fase 6: Despliegue (Fuera del alcance):** Esta fase implicaría la implementación del modelo en una aplicación real.

### 4. Estructura del Proyecto

```
nombre-del-repositorio/
│
├── 📁 preprocessed_data/      # Datos procesados listos para el modelado
│   ├── class_weights.pkl
│   ├── label_encoder.pkl
│   ├── X_test.npy
│   ├── X_train.npy
│   ├── X_val.npy
│   ├── y_test_cat.npy
│   ├── y_test_encoded.npy
│   ├── y_train_cat.npy
│   └── y_val_cat.npy
│
├── 📁 test.parquet/           # Datos originales de prueba
│
├── 📁 train.parquet/          # Datos originales de entrenamiento
│
├── 📜 CNN-Fase1-Fase2-Fase3.ipynb  # Notebook para fases 1, 2 y 3 (Comprensión y Preparación)
├── 📜 CNN-Fase4-Fase5.ipynb       # Notebook para fases 4 y 5 (Modelado y Evaluación)
└── 📄 README.md                   # Este archivo
```
*Nota: El archivo `best_model_v2.h5` con los pesos del modelo entrenado se genera en el directorio raíz al ejecutar el segundo notebook.*

### 5. Instalación

Para ejecutar este proyecto, se recomienda crear un entorno virtual y luego instalar las dependencias.

```bash
# 1. Clona el repositorio
git clone https://github.com/tu-usuario/nombre-del-repositorio.git
cd nombre-del-repositorio

# 2. Crea y activa un entorno virtual (opcional pero recomendado)
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# 3. Instala las librerías necesarias
pip install tensorflow pandas scikit-learn seaborn matplotlib numpy pyarrow
```

### 6. Uso

Los notebooks Jupyter deben ejecutarse en orden, ya que el segundo depende de los archivos generados por el primero.

1.  **Ejecutar `CNN-Fase1-Fase2-Fase3.ipynb`:** Este notebook cargará los datos originales desde las carpetas `parquet`, realizará el análisis exploratorio y guardará los arrays preprocesados en la carpeta `preprocessed_data/`.
2.  **Ejecutar `CNN-Fase4-Fase5.ipynb`:** Este notebook cargará los datos preprocesados, construirá, entrenará y evaluará la CNN, guardando el mejor modelo como `best_model_v2.h5` y generando todas las visualizaciones de resultados.

### 7. Arquitectura del Modelo

La CNN está diseñada con bloques convolucionales progresivos para extraer características jerárquicas, junto con técnicas de regularización para prevenir el sobreajuste.

| Capa                 | Configuración                                 | Propósito                                      |
| -------------------- | --------------------------------------------- | ---------------------------------------------- |
| `Rescaling`          | `1./255`                                      | Normalizar los píxeles al rango.        |
| `Conv2D` x 2 + `MaxPool` | 32 filtros, (3,3), `relu`                     | Extraer características de bajo nivel (bordes).    |
| `Conv2D` x 2 + `MaxPool` | 64 filtros, (3,3), `relu`                     | Extraer características de nivel medio (formas). |
| `Conv2D` x 2 + `MaxPool` | 128 filtros, (3,3), `relu`                    | Extraer patrones complejos.                    |
| `Flatten`            | -                                             | Convertir mapas 2D a un vector 1D.             |
| `Dense` + `Dropout`  | 512 neuronas, `relu`, Dropout(0.5)            | Capa de clasificación densa con regularización.  |
| `Dense` + `Dropout`  | 256 neuronas, `relu`, Dropout(0.4)            | Refinar la clasificación.                      |
| `Dense` (Salida)     | 4 neuronas, `softmax`                         | Producir probabilidades para cada clase.       |
| `BatchNormalization` | (Después de cada `Conv2D` y `Dense`)            | Estabilizar y acelerar el entrenamiento.       |

### 8. Resultados y Evaluación

El modelo final, evaluado en el conjunto de prueba, demostró un rendimiento excepcional con una **precisión general del 94.8%**.

#### Matriz de Confusión Normalizada (% Recall)
Esta matriz es clave, ya que muestra la sensibilidad del modelo para cada clase. El **Recall del 100% para 'Moderate Demented'** es el resultado más significativo, indicando que el modelo no clasificó erróneamente ningún caso de la etapa más severa.



#### Capacidad Discriminativa (Curva ROC - AUC)
Los valores de **AUC de 0.99 y 1.00** para todas las clases confirman que el modelo tiene una capacidad casi perfecta para distinguir entre las diferentes etapas de la enfermedad, validando su robustez frente al desbalanceo de clases.



### 9. Conclusiones

1.  **Alto Rendimiento Clínico:** El modelo no solo es preciso, sino también altamente sensible (`Recall`), especialmente para las clases más críticas, lo que lo convierte en una herramienta de diagnóstico potencialmente fiable.
2.  **Modelo Robusto:** La estrategia de usar pesos de clase manejó eficazmente el desbalanceo, lo que se confirma con las excelentes métricas de F1-score y AUC.
3.  **Entrenamiento Controlado:** El uso de callbacks como `EarlyStopping` y `ModelCheckpoint` fue fundamental para prevenir el sobreajuste y asegurar que el modelo final tuviera la mejor capacidad de generalización.
4.  **Superioridad de la CNN:** La arquitectura convolucional fue esencial para capturar las relaciones espaciales y los patrones sutiles en las RMN.

### 10. Trabajo Futuro

*   **Data Augmentation:** Aplicar técnicas de aumento de datos para incrementar la variabilidad del conjunto de entrenamiento y mejorar aún más la generalización.
*   **Transfer Learning:** Experimentar con arquitecturas pre-entrenadas (como VGG16, ResNet, o EfficientNet).
*   **Explainable AI (XAI):** Implementar técnicas como Grad-CAM para visualizar qué regiones de la RMN son más importantes para las predicciones del modelo.
*   **Despliegue:** Desarrollar una aplicación web simple (usando Streamlit o Flask) donde se pueda cargar una RMN y recibir una clasificación del modelo en tiempo real.
