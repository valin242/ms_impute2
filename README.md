# ms_impute2
 Benchmarking imputation methods for missing data in proteomics to assess their impact on downstream machine learning performance using tissue-specific protein intensity data.
# 🔬 Proteomics Missing Data Imputation & Machine Learning Evaluation 🧪

## Overview

This project evaluates the impact of different missing data imputation strategies on downstream machine learning model performance using a proteomics dataset (example: PXD010154 from PRIDE). It provides a workflow using R for initial data processing, missingness simulation, and basic imputation, followed by Python for advanced imputation, machine learning model training, and evaluation.

## Why is this Important? 🤔

Missing values are a pervasive challenge in proteomics, often arising from technical limitations (e.g., values below detection limits) or biological variability. The way these missing values are handled (i.e., the imputation method chosen) can significantly impact:

* **Data Distribution:** Altering the underlying structure of the data.
* **Statistical Analysis:** Biasing results like differential expression analysis.
* **Machine Learning Models:** Leading to inaccurate predictions, unreliable biomarker identification, and flawed biological interpretations.

Therefore, systematically evaluating different imputation methods, as done in this project, is **crucial** for understanding their effects and choosing appropriate strategies to ensure the robustness and reliability of downstream analyses, particularly in sensitive applications like disease classification or biomarker discovery based in Shaker Heights or anywhere else.

## 📊 Workflow

The analysis follows these main steps, transitioning from R to Python:

+---------------------------------+      +-----------------------------------+

|         R Script                |      |          Python Script            |

| (proteomics_r_script.R)         |      | (proteomics_ml_evaluation.py)   |

+---------------------------------+      +-----------------------------------+

|                                       ^

| 1. Load Raw Data                        | 5. Load R Output CSVs

| 2. QC & Normalize (MSnbase)             | 6. Load Labels/Metadata

| 3. Simulate Missingness (MCAR/MAR/MNAR) | 7. Advanced Imputation (MF, DAE)

| 4. Basic Imputation (Mean/Med/kNN/BPCA) | 8. Train/Eval ML (RF/MLP/XGB)|      

                             | 9. Visualize Results
                             
                  +-------------> Output CSVs ------------->+ 10. Output Results Table & Plots
                  
1.  **R Script (`proteomics_r_script.R`):**
    * Loads raw proteomics data (e.g., MaxQuant `proteinGroups.txt`).
    * Performs basic quality control (filtering contaminants/reverse sequences) and normalization (e.g., median normalization) using `MSnbase`.
    * Simulates missing data under three mechanisms: Missing Completely At Random (MCAR), Missing At Random (MAR), and Missing Not At Random (MNAR) at various proportions (10%, 20%, 30%).
    * Applies standard imputation methods: Mean, Median, k-Nearest Neighbors (kNN using `impute`), and Bayesian PCA (BPCA using `pcaMethods`).
    * Exports the normalized complete data, datasets with simulated missingness, and R-imputed datasets as CSV files.

## ⚙️ Prerequisites & Installation

### Software
* R (>= 4.0 recommended)
* Python (>= 3.8 recommended)

### R Packages
```R
# Install from CRAN
install.packages(c("BiocManager", "impute", "pcaMethods"))

# Install MSnbase from Bioconductor
BiocManager::install("MSnbase")
Python Librariespip install pandas numpy scikit-learn matplotlib seaborn missingpy torch xgboost
# Or using conda:
# conda install pandas numpy scikit-learn matplotlib seaborn pytorch xgboost -c conda-forge
# pip install missingpy # missingpy might need pip installation
(Note: Ensure CUDA is set up correctly if you intend to use GPU acceleration with PyTorch/XGBoost).💾 DataInput Data: The workflow is designed for processed proteomics quantification data, typically from search software like MaxQuant (proteinGroups.txt). The example dataset PXD010154 can be found on the PRIDE Archive. You will need to download the relevant processed file(s).Metadata: A separate sample metadata file (sample_metadata.txt or similar) is required. This file must map sample identifiers (matching the intensity column names in the data file, potentially after cleaning prefixes like "Intensity.") to experimental conditions or labels needed for supervised machine learning. It should be a simple text file (e.g., tab-separated).Example sample_metadata.csv:SampleName	Tissue Source
IntensitySample1l	Tissue 1
IntensitySample2	Tissue	1
IntensitySample3t	Tissue	2
IntensitySample4	Tissue	2
