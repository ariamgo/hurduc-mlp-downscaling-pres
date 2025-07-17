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
# Materials & Methods
Study area: Madrid, Spain

Sensor | Platform | Resolution | Role
--- | --- | --- | ---
SEVIRI | MSG | 15 min, 4.5 km | Input LST
VIIRS | Polar Orbiters | 2x/day, 750 m | Target LST
SLSTR | Polar Orbiters | 1 km | External Validation
MODIS EVI | Terra/Aqua | 500 m | Vegetation Input
Solar Zenith Angle | SEVIRI | - | Time-of-day proxy

---

# Model Architecture & Training

Pixel-wise MLP

- Inputs: SEVIRI, EVI, SZA
- 1 hidden layer, 5 neurons, tanh activation 
- Output: VIIRS LST 

For Madrid $\rightarrow \sim 6200$ models 


- Training: 2019–2022 (80% random samples)
- Test: 2019–2022 (20% hold-out)
- Independent validation: 2023–2024 (SNPP, JPSS1/2, S3A)
- Normalized inputs (−1, 1)
- Loss function: MSE

---
# Application and Evaluation

- Trained models applied to 2023–July 2024 at all 15-min SEVIRI intervals

- Performed external  validation (SNPP, JPSS1, JPSS2, and S3A)

- Also assessed:
    - Bias vs Land Cover Fraction
    - Diurnal variation across urban/rural/mixed sub-regions
    - Pixel-wise maps of RMSE, bias, and histograms of error

--

<figure>
  <img src="figures/train-test.png" alt="train test graph" style="width:70%">
  <figcaption>Estimated LST versus target LST: (a) training dataset, (b) test dataset</figcaption>
</figure>

--
<figure>
  <img src="figures/external validation.png" alt="external validation graph" style="width:100%">
  <figcaption>Estimated LST versus observed LST by sensor</figcaption>
</figure>

--
<figure>
  <img src="figures/external validation hist.png" alt="external validation hist" style="width:100%">
  <figcaption>Distribution of LST difference between estimated and observed LST by sensor.</figcaption>
</figure>

--
<figure>
  <img src="figures/bias rmse maps.png" alt="bias rmse maps" style="width:120%">
  <figcaption>Maps of bias (top) and RMSE (bottom) for each sensor</figcaption>
</figure>


---
# Strengths
- Simple, robust pixel-wise MLP
- Requires minimal auxiliary data
- Works well across multiple sensors
- Maintains 15-min temporal resolution
- Effective even with seasonal variability


# Weaknesses
- No spatial context (pixel-wise only)
    - Does not leverage shared structures to improve generalization
- No temporal memory
- Limited input features (no height, albedo, etc.)
- Underperforms in water/irrigated areas
- No uncertainty quantification
- Trained only on SNPP time range
- Scalability?
    - Domain specific
- Train-test evaluation only: not very robust

---
# Conclusions and outlook

A lightweight MLP per pixel can:
- Learn local spatio-temporal LST patterns
- Downscale from 4.5 km → 750 m with sub-degree error
- Generalize across sensors, seasons, and urban contexts

Can be improved, but already pretty good overall.

An elegant ML solution for real-world satellite data fusion.
