# Battery Cycle Life Prediction Using Enhanced $\Delta Q$ Variance Analysis

A comprehensive machine learning pipeline for predicting battery cycle life using early cycle data.


## Overview

This project implements a sophisticated approach to predict lithium-ion battery cycle life using only early cycling data (discharge curve of cycles 1 to 100). The methodology follows Severson et al.'s work on early prediction of battery cycle life, implementing feature extraction, polynomial fitting, and multiple modeling approaches including Bayesian uncertainty quantification. It enhances the single-feature 'variance' model of Severson et al. by adding robustness to the feature extraction, enabling uncertainty quantification, and providing an additional predictive feature to the battery cycle life prediction, representing a significant advancement in early battery lifetime assessment capabilities.


## The Core Idea

Instead of simply measuring capacity fade over time, the approach by Severson et al. analyzes how the shape of battery discharge curves evolves during cycling. As batteries degrade, their discharge curves become increasingly irregular and variable compared to early cycles. The variance of these shape changes serves as a powerful predictor of remaining battery life.


## Key Differences compared to Severson et al.

Rather than calculating the $\Delta Q_{100-10}$ variance from two isolated data points (which can be highly affected by measurement errors), this implementation analyzes the complete evolution of $\Delta Q$ variance from cycle 10 to 100.

**Original approach of Severson et al.'s single-feature 'variance' model:**
- Used single data points ($\Delta Q_{100-10}$ variance)
- Susceptible to measurement errors
- Only variance value as feature
- Single modeling approach

**This Implementation:**
- Polynomial fitting of the evolution of $\Delta Q$ variance (cycles 10-100) to capture the underlying degradation trend
- Extracts robust features at cycle 100 with uncertainty quantification
- Derives an additional feature in the form of the slope of variance evolution
- Error-Weighted Training: Models account for measurement uncertainty
- 6 different models with uncertainty quantification
- Comprehensive robustness comparison across models


## Pipeline Stages

1. **Data loading and preprocessing** - Batch merging and outlier cleaning following literature standards
2. **$\Delta Q$ variance feature extraction** - Computing variance features from cycling data
3. **Polynomial fitting and advanced feature engineering** - Robust feature extraction with uncertainty quantification
4. **Variance trajectory and feature quality visualizations** - Demonstrating polynomial fitting process and feature relationships
5. **Training 6 ML models** - Non-fitted $\Delta Q$ variance baseline, fitted $\Delta Q$ variance feature, slope integration, error weighting, and Bayesian uncertainty quantificatio
6. **Model performance comparison and error analysis visualizations** - Comprehensive evaluation across multiple datasets


## Model Comparison

The implementation includes 6 different models for comprehensive comparison:

1. **Non-Fit $\Delta Q$ Variance:** Directly utilizes the $\Delta Q_{100-10}$ variance between cycles 100 and 10 (baseline ≈ Severson et al.)
2. **$\Delta Q$ Variance:** Polynomial-fitted $\Delta Q_{n-10}$ at cycle 100 for enhanced robustness
3. **$\Delta Q$ Variance & Slope:** Combined variance and slope features from polynomial fit for additional predictive power
4. **$\Delta Q$ Variance + Error:** Error-weighted training with fitted variance feature for uncertainty-aware learning
5. **$\Delta Q$ Variance & Slope + Error:** Combined features from fit with error weighting for maximum information utilization
6. **Bayesian Error Model:** Full uncertainty propagation of fitted variance feature through MCMC sampling


## Available Versions

**Main Version (`cycle_life_prediction.ipynb`):**
- Complete interactive Jupyter notebook with comprehensive markdown documentation
- User choice between deterministic (reproducible) and non-deterministic execution modes
- Integrated caching system for faster reruns
- Progressive 9-step pipeline with detailed progress tracking
- All 6 models with comprehensive explanations

**Basic Version (`cycle_life_prediction_basic.ipynb`):**
- Streamlined implementation without interactive features
- Standard random initialization only
- Minimal documentation for quick execution
- Ideal for automated runs or integration into other workflows


## Usage

### Main Interactive Version
Run all cells in sequence. The pipeline will:
- **Prompt for deterministic vs non-deterministic execution mode** for reproducible results
- **Check for cached data and offer option to skip reprocessing**, enabling much faster reruns (skips Steps 1-4)
- **Download battery data automatically** if not present locally
- **Execute all 9 pipeline stages with progress tracking**
- **Generate comprehensive visualizations and performance metrics**

### Basic Version
1. Open `cycle_life_prediction_basic.ipynb` in Jupyter
2. Run all cells for streamlined execution without interactive features


## Visualization and Results

**Step 4: Variance Trajectory Visualization**
Shows the polynomial fitting process for individual battery cells, demonstrating:
- Scattering in single $\Delta Q_{n-10}$ variance data points
- How polynomial fitting creates more robust features through outlier removal and uncertainty quantification
- Evaluation at cycle 100 with clear improvement in feature quality through the fitting process

**Step 5: Feature Quality Assessment**
Displays cycle life vs. variance relationships similar to the original Severson paper:
- Left plot shows traditional variance vs. cycle life relationship
- Right plot shows new slope feature demonstrating similar predictive behavior
- Transparency encoding visualizes relative error (more transparent points indicate higher uncertainty)
- Demonstrates strong correlation between both features and battery cycle life

**Step 7: Model Performance Visualizations**
- Predicted vs. True scatter plots demonstrating good agreement across all models
- Residual histograms showing distribution of prediction errors
- Validation across multiple datasets (train, primary test, and secondary test)
- Format similar to original paper for direct comparison

**Step 8: Model Robustness Comparison**
- Inter-dataset error analysis showing performance consistency across different test sets
- Enhanced models demonstrate overall improvements compared to the non-fitting baseline, which is based on the methodology of Severson et al.
- Lower prediction errors, particularly for uncertainty-aware models
- Visual gradient background indicating relative model reliability
- Ranked performance summary table with robustness index


## Key Results

**Improved Robustness:** Enhanced models reduce the prediction error of the Primary Test dataset while maintaining comparable performance on the Secondary Test dataset, indicating more balanced generalization.


## Requirements

- **Core scientific computing:** numpy, pandas, matplotlib, scipy
- **Machine learning:** tensorflow, scikit-learn
- **Bayesian modeling:** pymc, arviz
- **Data handling:** h5py
- **Progress tracking:** tqdm


## Dataset

Uses the publicly available Stanford/MIT battery dataset containing:
- 124 commercial lithium-ion batteries
- Multiple cycling protocols and environmental conditions
- Complete voltage/capacity cycling data for each cell
- Validated cycle life measurements for ground truth


## References

**Original Work:**
Severson, K.A., Attia, P.M., Jin, N. et al. Data-driven prediction of battery cycle life before capacity degradation. Nat Energy 4, 383–391 (2019). https://doi.org/10.1038/s41560-019-0356-8

**Data availability:**
https://data.matr.io/1/


## License

MIT License


## Contact

hesslingjanis@gmail.com  
https://orcid.org/0009-0009-1312-1278
