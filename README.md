# CSE163_project
# Regional Validity of the Traditional Solar Term System  
_A Comparative Climate and Air Quality Analysis between Northern China and Korea_

## Project Overview

This repository contains the final project for **CSE 163: Intermediate Data Programming** at the University of Washington.  
The project investigates how well the traditional **24 solar term** system (절기) matches **modern climate and air-quality patterns** in **Seoul (Korea)** and **Beijing (China)**, using real-world weather and PM2.5 datasets and regression models implemented in Python.

All analysis, figures, and results are implemented in the Jupyter Notebook:

- `project.ipynb` – the main notebook containing:
  - project description and motivation  
  - data loading and cleaning  
  - feature engineering  
  - modeling and evaluation (RQ1–RQ3)  
  - discussion, impact, limitations, and testing  


---

## Research Questions & Main Findings

### **RQ1. How accurately do the traditional 24 solar terms reflect modern seasonal changes?**

- **Goal:** Evaluate how strongly the 24 solar terms help explain **daily temperature** variation in Seoul.  
- **Method:** Train regression models predicting daily average temperature (`Temp`) from meteorological features plus `SolarTerm`.  
- **Finding:**  
  The solar term system has **only weak predictive value** for modern temperature.  
  Continuous astronomical indicators (sunrise/sunset time) dominate seasonal variation.  
  Solar terms capture only a **coarse secondary seasonal trend**.

---

### **RQ2. How do seasonal climate patterns differ between Seoul and Beijing through the lens of solar terms?**

- **Goal:** Compare the importance of `SolarTerm` in temperature prediction models for both cities.  
- **Method:** Train identical feature sets and models for Seoul and Beijing.  
- **Finding:**  
  Solar terms align **more strongly with Beijing’s** climate.  
  The system historically originated for **northern China’s continental climate**, which matches Beijing better than Seoul’s maritime-influenced climate.

---

### **RQ3. Can the solar term system be applied to modern environmental phenomena such as air pollution?**

- **Goal:** Predict PM2.5 using meteorological features + `SolarTerm`.  
- **Method:** Train regression models for both cities.  
- **Finding:**  
  Solar terms provide **virtually no predictive power** for PM2.5.  
  Air quality depends primarily on weather conditions and human activity (industry, heating, emissions), not traditional seasonal divisions.

---

## Data Sources

### **1. Seoul Weather (1994–2024)**  
- Daily temperature, humidity, precipitation, pressure, wind, cloud cover, visibility  
- Includes sunrise, sunset, and moon phase  
- Used for RQ1 and RQ3

### **2. Seoul Air Pollution (2017–2019)**  
- Hourly PM2.5 measurements (aggregated to daily)  
- Merged with Seoul daily weather data

### **3. Beijing Weather & PM2.5 (2010–2015)**  
- Daily meteorological data and PM2.5 (`PM_US Post`)  
- Wind direction originally categorical; converted to degrees and vector components  
- Used for RQ2 and RQ3  

---

## Methods

### **Data Loading & Standardization**

Key functions (in `project.ipynb`):

- `Get_Seoul_Weather_data()`  
  Loads multiple CSVs, standardizes column names, converts temperature units, parses dates.

- `Get_Seoul_PM25_data()`  
  Loads PM2.5 data, filters to station 101, computes daily averages.

- `Get_Merged_Seoul_data()`  
  Merges weather + air pollution by date.

- `Get_Beijing_data()`  
  Aggregates Beijing PM and weather data, maps wind direction labels, normalizes features.

---

### **Solar Term Feature Engineering**

- `Add_SolarTerm(df)`  
  Adds categorical 24-season label based on date.

- `solarterm_to_cos(series)`  
  Encodes the 24 terms as a smooth cyclic cosine value.

Additional helper transforms:

- `normalize(series)`  
- `cap_and_normalize(series, upper_bound)`  
- `winddir_to_cos(series)`  
- `extract_time_cos(series)` (sunrise/sunset)  
- `moonphase_to_cos(series)`  

Each RQ uses tailored feature sets combining these transformations.

---

### **Models Used**

All models implemented using **scikit-learn**:

- `LinearRegression`
- `RandomForestRegressor`
- `GradientBoostingRegressor`

Evaluation metrics:

- **R² score**  
- **RMSE**  
- **Feature importances** (coefficients or tree-based feature_importances)

Plots in the notebook compare importance values and highlight `SolarTerm`.

---

## Repository Structure (expected)

- CSE163_project/  
  - project.ipynb — Main analysis notebook  
  - Seoul_Weather_dataset/ — Raw Seoul weather CSV files  
  - Seoul_Air_Pollution_dataset/ — Seoul PM2.5 data  
  - Beijing_Weather_PM_dataset/ — Beijing weather + PM2.5 data  
  - README.md — This documentation file  

---

## How to Run

1. Download or clone the repository and place datasets in the expected directories.

2. Create a Python environment:

   ```bash
   conda create -n cse163_project python=3.10
   conda activate cse163_project
   ```

3. Install dependencies:

   ```bash
   pip install pandas numpy matplotlib scikit-learn jupyter
   ```

4. Launch Jupyter:

   ```bash
   jupyter lab
   ```

5. Open **project.ipynb** and run all cells from top to bottom.

---

## Testing

The notebook includes assert-based tests that verify:

- Correct functionality of helper functions  
- Accurate solar term label assignment  
- Proper loading and merging of datasets  
- Stability of model evaluation utilities  

All tests pass, confirming correctness of the pipeline.

---

## Impact & Limitations

### Impact
- Demonstrates how traditional seasonal markers (24 solar terms) align with modern climate patterns.
- Reveals regional differences between Seoul and Beijing through statistical modeling.
- Shows why solar terms are not effective predictors of PM2.5 compared to meteorological variables.

### Limitations
- PM2.5 datasets have shorter time spans than weather datasets.
- Observational data restricts causal interpretation.
- Solar term encoding simplifies a historically and culturally rich system.

---

## Author

**Ryan Choe — University of Washington**  
CSE 163 Final Project (Autumn 2025)
