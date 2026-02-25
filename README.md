# Hyperspectral PET Identification using NEWTEC BUTEO

## Overview

This project evaluates hyperspectral imaging (HSI) for plastic identification with a focus on polyethylene terephthalate (PET).

A two-stage machine learning pipeline is implemented:

1. Plastic vs. Non-Plastic classification  
2. PET vs. Other Plastics classification  

Models are trained on public hyperspectral datasets and applied directly to spectra recorded using the NEWTEC BUTEO system without retraining.

---

## System and Hardware

Validation data was acquired using:

- NEWTEC BUTEO hyperspectral imaging system  
- OCULUS hyperspectral camera  
- Spectral range: 430–1000 nm  
- 330 spectral bands  
- Pushbroom (line-scan) acquisition  

Measurements are exported as `.pam` hyperspectral cubes and converted to reflectance spectra.

---

## Datasets (Version 3)

### Training Data

- Public hyperspectral plastics dataset (Virgin Plastics dataset)

All spectra are harmonized to match the NEWTEC wavelength grid (430–1000 nm, 330 bands).

### Validation Data

- Spectra acquired with the NEWTEC BUTEO system  
- Manual ROI selection  
- Reflectance calibration using white and dark references  

---

## Preprocessing

- Reflectance calibration  
- Wavelength interpolation to 430–1000 nm (330 bands)  
- Feature standardization (StandardScaler fitted on training data)  

All preprocessing components are saved and reused during inference.

---

## Model Architecture

Two-stage binary classification pipeline:

### Stage 1  
Plastic vs. Non-Plastic

### Stage 2  
PET vs. Other Plastics

Both stages use:

- Support Vector Machines (SVM)  
- RBF kernel  
- Probabilistic outputs for threshold-based decisions  

SVMs were selected for robustness with high-dimensional spectral data and efficient inference.

---

## Inference Pipeline

For each exported spectrum:

1. Interpolate to common wavelength grid  
2. Apply saved scaler  
3. Stage 1: Estimate P(plastic)  
4. Stage 2 (if plastic): Estimate P(PET)  
5. Apply probability thresholds for final decision  

No retraining is performed on NEWTEC data.

---

## Results

### Public Dataset

- PET vs. PE accuracy: 95%  

### NEWTEC Validation

- PETG sample classified as plastic with high confidence  
- PET probability ≈ 0.88 across tested ROIs  
- Consistent predictions despite amplitude and noise differences  

The pipeline demonstrates stable cross-system transfer using spectral harmonization.

---

## Repository Structure

```
MDB2/
├── data/
│   ├── Full_Data/
│   │   └── hyperspectral_database/
│   ├── Mylab/
```

Version 3 uses the Virgin Plastics hyperspectral dataset.
