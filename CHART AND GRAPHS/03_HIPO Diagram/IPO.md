
---

### Module 1.0: Data Acquisition

| Input | Process | Output |
| :--- | :--- | :--- |
| • Multi-year raw Sentinel-2 bands *(Copernicus Hub)*<br>• Reference mangrove boundary maps *(GMW / PhilSA)*<br>• Planting records & field boundary updates *(LGU / DENR)* | 1. Query Copernicus Hub for multi-year satellite imagery.<br>2. Ingest reference ground truth maps.<br>3. Collect local ground truth planting and cutting records from LGU/DENR.<br>4. Store raw raster files into Data Store D1. | • Raw satellite image rasters *(Stored in D1)*<br>• Raw reference label masks |

---

### Module 2.0: Image Preprocessing (Detailed)

| Input | Process | Output |
| :--- | :--- | :--- |
| • Raw Sentinel-2 image files *(D1)*<br>• Raw reference label maps *(Module 1.0)* | **2.1 Band Stacking:** Stack multi-spectral satellite bands.<br>**2.2 NDVI Calculation:** Compute Normalized Difference Vegetation Index: `(NIR - Red) / (NIR + Red)`.<br>**2.3 Normalization:** Scale pixel intensities to a 0–1 continuous range.<br>**2.4 Format & Project Labels:** Re-project label maps to match image coordinate reference system (CRS).<br>**2.5 Generate Patches & Align Masks:** Extract 256x256 sub-image patches and align label masks. | • Stacked 5-channel image tensors<br>• Full preprocessed annual imagery *(Stored in D2)*<br>• 256x256 training image patches & label masks *(Stored in D2)* |

---

### Module 3.0: U-Net Model Training

| Input | Process | Output |
| :--- | :--- | :--- |
| • 256x256 training patches & label masks *(D2)*<br>• Network hyperparameters *(Epochs, Batch Size, Learning Rate)* | 1. Ingest image patches and paired binary ground-truth masks.<br>2. Train the U-Net convolutional neural network architecture.<br>3. Monitor validation loss and evaluate accuracy metrics.<br>4. Save the optimal neural network checkpoint weights. | • Trained U-Net model weight checkpoint (`.h5` / `.pth` stored in D3) |

---

### Module 4.0: Per-Year Prediction

| Input | Process | Output |
| :--- | :--- | :--- |
| • Full preprocessed annual imagery *(D2)*<br>• Trained U-Net model weights *(D3)* | 1. Load trained U-Net model weights.<br>2. Pass full preprocessed multi-temporal satellite scenes through the model.<br>3. Generate continuous probability map patches (0.0 – 1.0).<br>4. Apply decision threshold (0.50) to create binary mangrove classifications. | • Annual binary mangrove masks *(2019, 2021, 2023, 2024 stored in D4)* |

---

### Module 5.0: Temporal Change Detection

| Input | Process | Output |
| :--- | :--- | :--- |
| • Baseline binary mask ($T1$, e.g., 2019) *(D4)*<br>• Target binary mask ($T2$, e.g., 2024) *(D4)* | 1. Load binary masks for selected years $T1$ and $T2$.<br>2. Perform pixel-by-pixel spatial comparison to derive 4 change classes:<br>&nbsp;&nbsp;• Unchanged Non-Mangrove (0)<br>&nbsp;&nbsp;• Stable Mangrove (1)<br>&nbsp;&nbsp;• Mangrove Loss (2)<br>&nbsp;&nbsp;• Mangrove Gain (3)<br>3. Convert pixel counts to hectares (`Count × 0.01 ha`).<br>4. Calculate net gain/loss metrics (`Gain - Loss`). | • 4-class spatial change matrix raster *(Stored in D5)*<br>• Quantified surface area statistics in hectares *(Stored in D5)* |

---

### Module 6.0: Ground Truth Validation

| Input | Process | Output |
| :--- | :--- | :--- |
| • Generated change maps *(Module 5.0)*<br>• Planting records, cutting logs, and field reports *(LGU / DENR)* | 1. Overlay spatial change maps with local field verification data.<br>2. Compare detected gain/loss boundaries against ground truth planting/cutting logs.<br>3. Log false positives and misclassifications.<br>4. Provide feedback to refine training data and label maps. | • Refined validation feedback<br>• Updated training labels for acquisition pipeline |

---

### Module 7.0: Web Application & Reporting

| Input | Process | Output |
| :--- | :--- | :--- |
| • Change maps & hectare statistics *(D5)*<br>• User year selection query *(Baseline T1, Target T2)* | 1. Process user year selection queries via the web interface.<br>2. Query Data Store D5 for matching spatial change maps and hectare metrics.<br>3. Render interactive multi-layer map overlays (Stable, Loss, Gain).<br>4. Display dynamic charts and net change statistical summary card.<br>5. Compile spatial maps and statistical tables into an exportable PDF document. | • Interactive GIS web dashboard<br>• Exported summary PDF report |
