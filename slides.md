# A Multi-Layer Perceptron Approach to Downscaling Geostationary Land Surface Temperature in Urban Areas

Alexandra Hurduc, Sofia L. Ermida, and Carlos C. DaCamara

Maria Gkolemi, 17/7/2025

---

# Introduction

LST = key variable for urban climate & heat island studies

Remote sensing trade-off:
- Geostationary: High temporal, low spatial resolution
- Polar orbiters: High spatial, low temporal resolution

Goal: learn a mapping from coarse-frequent data to fine-sparse data ✨Downscaling✨

In this study: Use a Multi-Layer Perceptron (MLP) to downscale SEVIRI geostationary LST (4.5 km) to 750 m using VIIRS data as target.

---
# Data
Sensor | Platform | Resolution | Role
--- | --- | --- | ---
SEVIRI | MSG | 15 min, 4.5 km | Input LST
VIIRS | Polar Orbiters | 2x/day, 750 m | Target LST
SLSTR | Polar Orbiters | 1 km | External Validation
MODIS EVI | Terra/Aqua | 500 m | Vegetation Input
Solar Zenith Angle | SEVIRI | - | Time-of-day proxy

---

# Model Architecture

Pixel-wise MLP
Inputs: SEVIRI, EVI, SZA
1 hidden layer, 5 neurons, tanh activation

For Madrid $\rightarrow \approx 6200$ models 

---
# Training

- Training: 2019–2022 (80% random samples)
- Test: 2019–2022 (20% hold-out)
- Independent validation: 2023–2024 (SNPP, JPSS1/2, S3A)
- Normalized inputs (−1, 1)
- Loss function: MSE

---
# Results

---
# Strengths
✅ Simple, robust pixel-wise MLP
✅ Requires minimal auxiliary data
✅ Works well across multiple sensors
✅ Maintains 15-min temporal resolution
✅ Effective even with seasonal variability

--
# Weaknesses
❌ No spatial context (pixel-wise only)
❌ Limited input features (no height, albedo, etc.)
❌ Underperforms in water/irrigated areas
❌ No uncertainty quantification
❌ Trained only on SNPP time range
❌ Computational inefficiency for large areas