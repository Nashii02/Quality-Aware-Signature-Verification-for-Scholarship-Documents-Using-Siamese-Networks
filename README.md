# Mangrove Gain and Loss Detection System

A multi-temporal deep learning and geospatial analytics web application designed to track, segment, and quantify mangrove cover dynamics (Gain, Loss, and Persistence) using Sentinel-2 satellite imagery and U-Net architecture.

---

## Project Overview

Mangrove ecosystems serve as critical coastal defense mechanisms and carbon sinks, yet they face severe degradation from environmental stressors and human encroachment. This project provides coastal resource management officers (e.g., MENRO / DENR) with an end-to-end automated platform to:

* **Ingest Multi-Spectral Imagery:** Process multi-temporal 4-band Sentinel-2 GeoTIFF rasters with calculated NDVI index layers.
* **Perform Deep Learning Segmentation:** Utilize a lightweight U-Net convolutional neural network trained on normalized 256 x 256 spatial patches.
* **Detect Spatiotemporal Changes:** Quantify categorical shifts (Unchanged, Stable Mangrove, Mangrove Loss, Mangrove Gain) and aggregate area dynamics in hectares 
* **Visualize & Export Results:** Display interactive web-based map overlays, dynamic statistical dashboards, and downloadable PDF reports for decision support.

---

## Tech Stack

| Category | Purpose | Tool | Why |
| --- | --- | --- | --- |
| **Deep Learning & Data Processing** | U-Net model | **TensorFlow / Keras** | Most tutorials available, beginner-friendly, well-documented. |
|  | Satellite image handling | **Rasterio** | Standard library for reading `.SAFE` Sentinel-2 files. |
|  | Data manipulation | **NumPy** | Required for band stacking, patch creation, change detection. |
|  | Label / shapefile handling | **GeoPandas** | For working with GMW shapefiles and mangrove masks. |
|  | Visualization of results | **Matplotlib** | Quick plots during development and model evaluation. |
|  | Training environment | **Google Colab** | Free GPU — laptops likely can't train U-Net efficiently. |
| **Web Application** | Backend | **Flask (Python)** | Same language as ML code — no context switching. |
|  | Interactive map | **Leaflet.js** | Free, lightweight, perfect for displaying GeoTIFF/GeoJSON layers. |
|  | Statistics charts | **Chart.js** | Simple, well-documented, works with Leaflet cleanly. |
|  | Frontend styling | **Bootstrap 5** | Fast to build, responsive, no need for a separate CSS framework. |
|  | PDF report generation | **ReportLab** | Python-based, generates downloadable reports directly from Flask. |
|  | Map layer format | **GeoJSON / GeoTIFF** | Standard geospatial formats Leaflet handles natively. |
| **Supporting Tools** | Satellite image download | **Copernicus Data Space** | Level-2A Sentinel-2 — free. |
|  | Image preprocessing & labeling | **QGIS** | Free GIS software, verify and correct training labels. |
|  | Version control | **GitHub** | Coordinate code between group members. |
|  | Model accuracy metrics | **scikit-learn** | IoU, F1, confusion matrix — one import away. |

---

### 💡 Tech Stack Rationale

> **Why Flask over Django?**
> Django is too heavy for what the system needs — it's built for large-scale web apps. Flask is minimal, and since the entire pipeline is already in Python, the ML model and web server live in the same codebase. One language, one environment, no translation layer.

> **Why Bootstrap 5 + Leaflet.js over React/Vue?**
> Bootstrap 5 with vanilla JavaScript and Leaflet.js gives a clean, functional interface without the learning curve of a JavaScript framework.

---

## 🏗️ System Architecture & Workflow

```text
Sentinel-2 imagery (Copernicus)
        ↓
QGIS — label verification
        ↓
Python preprocessing (rasterio + NumPy + GeoPandas)
        ↓
U-Net training on Google Colab (TensorFlow / Keras)
        ↓
Saved model (.h5 file)
        ↓
Flask backend
        ↓
Leaflet.js frontend (change map + Chart.js stats + PDF download)
        ↓
MENRO officer views results in browser

```

---


## 📁 Repository Structure

```text
├── CHART AND GRAPHS/
│   ├── 01_DFD/
│   │   ├── stage_0.md
│   │   ├── stage_1.md
│   │   └── stage_3.md
│   ├── 02_Structured_Chart/
│   │   └── CamScanner 07-24-2026 20.00.jpg
│   ├── 03_HIPO Diagram/
│   │   ├── Hierarchy_Chart.md
│   │   └── IPO.md
│   ├── 04_Structured_English/
│   │   ├── Module1_Data_Acquisition_&_Preprocessing_Engine
│   │   ├── Module2_U-Net_Deep_Learning_Segmentation_Engine
│   │   ├── Module3_Temporal_Change_Detection_Engine
│   │   └── Module4_Web_Dashboard_&_Reporting_Subsystem
│   ├── 05_Pseudo_Code/
│   │   ├── Pseudocode1_Data_Acquisitio_&_Preprocessing_Engine
│   │   ├── Pseudocode2_U-Net_Deep_Learning_Segmentation_Engine
│   │   ├── Pseudocode3_Temporal_Change_Detection_Engine
│   │   └── Pseudocode4_Web_Dashboard_&_Reporting_Subsystem
│   ├── 06_ERD/
│   │   └── ERD.md
│   └── 07_Data_Dictionary/
│       └── data_dictionary.csv
├── DATASET/
│   ├── Test/
│   └── Train/
├── Main/
├── Related Articles/
└── README.md

```

---
