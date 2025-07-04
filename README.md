# 🌲 Forest Fire Prediction and Spread Simulation using AI/ML 

This project which involves:
- Predicting next-day forest fire risk zones using machine learning.
- Simulating fire spread across hours (1/2/3/4/6/12) using terrain, weather, and fuel data.

---

## 🔥 Problem Statement Overview

Uncontrolled forest fires threaten biodiversity, climate, and human settlements. Their spread depends on weather, terrain, and land-use. While satellite datasets are available, **near real-time simulation and forecasting remain a challenge**.

**Objective:**
1. Predict forest fire risk map (Low, Moderate, High) for Day N+1.
2. Simulate fire spread over time (1, 2, 3, 4, 6, 12 hours) from high-risk zones.

---

## 🧠 Technologies Used

| Category         | Tools / Frameworks |
|------------------|--------------------|
| Programming      | Python 3.11+       |
| ML/DL Models     | Random Forest (Baseline), U-Net (Deep Learning), Cellular Automata (Fire Spread) |
| Data Processing  | NumPy, Pandas, Rasterio |
| GIS Tools        | QGIS, Google Earth Engine |
| Visualization    | Matplotlib, ImageIO |
| IDE/Platform     | Google Colab       |

---

## 📦 Dataset Used

| Type          | Source                          | Description                          |
|---------------|----------------------------------|--------------------------------------|
| DEM           | Bhoonidhi Portal                | Used to derive slope/aspect          |
| LULC          | Bhuvan (ISRO) / Earth Engine    | Indicates fuel availability          |
| Weather       | ERA5 / MOSDAC / IMD             | Temperature, Wind, Rain, Humidity    |
| Human Sett.   | GHSL                            | Stressor layer: human-built areas    |
| Fire Labels   | VIIRS-SNPP Active Fires         | Ground truth for training            |

All raster data was **resampled to 30m resolution** and aligned.

---

## 🔧 Project Workflow

### 🔹 Step 1: Data Preprocessing
- All raster datasets (DEM, LULC, weather) are reprojected, resampled, and stacked into a multiband `.tif`.
- Labels created from **VIIRS fire detections** as multi-class fire risk (0: No Fire, 1: Low, 2: Moderate, 3: High).

### 🔹 Step 2: Fire Risk Prediction

#### ✅ A. Random Forest (Baseline)
- Input features were extracted from raster bands (pixel-wise vectors).
- Labels were fire risk classes (0, 1, 2, 3).
- Model trained using `scikit-learn` RandomForestClassifier.
- Predicted map reshaped back to original raster shape.
- Helped evaluate feature importance and provided fast prototyping.

#### ✅ B. U-Net (Deep Learning)
- Input: 10-band raster patches (128×128).
- Output: Segmentation mask of fire risk classes.
- Trained for 20+ epochs using class-weighted loss to handle imbalance.
- Output: `predicted_fire_risk_map.tif` (classes: Low, Moderate, High)

### 🔹 Step 3: Fire Spread Simulation using Cellular Automata
- High-risk pixels are used as fire seeds.
- Fire spreads per hour based on:
  - Wind vectors (U/V)
  - Slope & terrain
  - LULC (fuel availability)
  - Spread probability threshold
- Output: Binary fire maps for Hour-1 to Hour-12.

### 🔹 Step 4: Visualization
- `plot_risk_and_spread_grid()` overlays predicted risk and spread maps on LULC/terrain.
- Color-coded risk levels (gray-yellow-red) and fire overlays (red).
- Output: Final grid visualization and optional animation.

---

## 📊 Final Output

- ✅ `predicted_fire_risk_map.tif` → 30m raster of fire risk for Day N+1.
- ✅ `fire_points_binary.tif`, ... → Binary fire spread maps.
- ✅ Combined grid of prediction and hourly fire spread.

---



