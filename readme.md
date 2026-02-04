# English Premier League Logo Detection (20K) – Dataset Exploration (EDA)

## Exercise Summary

Este proyecto realiza una **exploración concisa del dataset** *English Premier League Logo Detection (20K images)* con foco en entender su estructura: **tamaño del dataset**, **distribución de clases**, **estructura de carpetas**, **dimensiones y canales de imágenes (muestreo)**, **ejemplos visuales por clase**, y **recomendaciones de preprocesamiento** (RGB, resize, normalización y augmentations).  
El objetivo es comprensión y preparación del pipeline, no estadística exhaustiva.

## Introducción

El objetivo de este notebook es dejar el dataset listo para entrenamiento de modelos de visión (clasificación de logos por equipo).  
Me enfoqué en:

- Entender cómo está organizada la data en disco (rutas locales vs rutas “Kaggle style” del CSV).
- Ver si el dataset está balanceado y cuántas clases reales hay.
- Identificar tamaños y canales típicos para definir **resize** y **normalización**.
- Mostrar ejemplos por clase para validar que las etiquetas corresponden.

## Dataset Description

**Kaggle – English Premier League Logo Detection (20K images)**  
Dataset: `alexteboul/english-premier-league-logo-detection-20k-images`

- **Total de imágenes**: 20,000
- **Estructura detectada**:
  - Carpeta de imágenes: `epl-logos-big/...` (con subcarpetas por equipo)
  - Archivo de etiquetas: `train.csv`
- **Etiquetas**:
  - `team_name` (string)
  - `team` (id numérico)
- **CSV**:
  - **Filas**: 20,000
  - **Columnas**: `filepath`, `team_name`, `team`
  - Nota: `filepath` viene con ruta estilo Kaggle (`../input/...`) y se resuelve a rutas locales con la columna `local_path`.

---

## Qué hay en el notebook

### `EDA_DATASET_EXPLORATION.ipynb`

#### **1.2 – Inspección de estructura del dataset**
- Verificación de existencia del path local.
- Listado de entradas en raíz (carpetas/archivos).
- Búsqueda **recursiva** de imágenes (`.png/.jpg/.jpeg/...`).
- Identificación de subcarpeta candidata con mayor cantidad de imágenes.

**Resultado observado:**
- 20,000 imágenes encontradas recursivamente.
- `train.csv` localizado en la raíz del dataset.
- Imágenes dentro de `epl-logos-big/...` (con doble nivel `epl-logos-big/epl-logos-big/...`).

#### **1.3 – Distribución de clases (labels)**
- Carga de `train.csv`.
- Gráfica **Top N vs Bottom N** por cantidad de imágenes por clase (`team_name`).
- Validación rápida de consistencia (si aplica): que `team_name` mapee correctamente a un `team_id`.

Outputs principales:
- Conteos por clase (`team_name`) y shape global del dataset (20,000 filas).

#### **1.4 – Dimensiones y canales de imágenes (muestreo)**
- Muestreo de `N` imágenes para extraer:
  - `width`, `height`
  - `mode` (`RGB`, `RGBA`, etc.)
- Histogramas de distribución de ancho y alto.
- Conteo de modos para detectar necesidad de convertir a RGB.

#### **1.5 – Ejemplos por clase**
- Grid con `K` clases (top por frecuencia) y `M` imágenes por clase.
- Verificación visual rápida de:
  - Calidad de imágenes
  - Consistencia label ↔ contenido (logo del equipo)
  - Variabilidad de fondos / tamaños / recortes

#### **1.6 – Recomendaciones de preprocesamiento**
Checklist de preparación típica para entrenamiento:
1. Convertir imágenes a **RGB** (si hay `RGBA`).
2. **Resize** a tamaño fijo (ej: `224x224` o `256x256` según backbone).
3. Normalización:
   - Simple: `x / 255.0`
   - Transfer learning (ImageNet): mean `[0.485,0.456,0.406]`, std `[0.229,0.224,0.225]`
4. Augmentation (train): random resize/crop, flips (si aplican), color jitter leve.
5. Split estratificado por clase.

---

## Requisitos

- Python 3.10+ (recomendado)
- Librerías:
  - `pandas`
  - `numpy`
  - `matplotlib`
  - `Pillow`

---

## Información del Proyecto

- **Autor**: Esteban Aguilera Contreras
- **Universidad**: Escuela Colombiana de Ingeniería Julio Garavito
- **Asignatura**: Arquitecturas Empresariales (AREP)