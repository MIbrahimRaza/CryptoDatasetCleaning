# Crypto Dataset Cleaning

A complete data cleaning and preprocessing pipeline applied to a messy, multi-cryptocurrency OHLCV dataset.

---

## Dataset

Daily market data for multiple cryptocurrencies (e.g. Bitcoin, Axie Infinity, Aave), with the following raw columns:

`Name`, `date`, `symbol`, `open`, `high`, `low`, `close`, `Price Change`, `Volume Coin`, `Volume USD`, `BlockChain Type`, `Sentiment`

The raw data contains common real-world data quality issues: missing values, inconsistent/placeholder strings, negative values where they shouldn't exist, and incomplete daily timelines per coin. Provided as `Datasets.rar`.

---

##  Cleaning & Preprocessing Pipeline

The notebook walks through a full pipeline, step by step:

**1. Structural Cleaning**
- Converting numeric-like columns stored as text into proper numeric types
- Detecting and fixing negative values
- Standardizing inconsistent categorical values (`BlockChain Type`, `Symbol`, `Name`)
- Converting placeholder strings (e.g. `"??"`, `"###"`) into true `NaN` values

**2. Timeline Reconstruction**
- Rebuilding a complete daily timeline for each coin (filling in missing dates) and validating the result

**3. Missing Value Imputation**
- Logical filling for categorical fields (`Name`, `Symbol`, `BlockChain Type`)
- Comparing four imputation techniques for numeric fields — **Linear Interpolation**, **KNN Imputation**, **Linear Regression Imputation**, and **Mean Imputation** — evaluated using RMSE and MAE

**4. Outlier Detection & Removal**
- Comparing three outlier-handling methods (**IQR**, **Z-Score**, **Winsorization**) on real data to evaluate trade-offs between data loss and distortion
- Applying **IQR-based outlier removal** per coin (so one coin's volatility doesn't distort another's)

**5. Hybrid Adaptive Smoothing**
- For each numeric column, comparing **Regression Smoothing** vs. **Moving Average (MA)** and dynamically selecting whichever achieves greater noise reduction

**6. Winsorization + Log Transformation**
- Capping extreme values at the 1st/99th percentile (post-smoothing)
- Applying log transformation to stabilize variance and reduce skewness

**7. Normalization**
- Comparing **Min-Max Scaling** vs. **Z-Score Standardization**
- Applying **Min-Max Normalization** as the final step (better suited to skewed, non-normally-distributed financial data)

**8. Exploratory Data Analysis**
- Univariate analysis (histograms with KDE, boxplots) per coin to study distribution, skewness, and remaining outliers
- Market trend and sentiment impact analysis

---

##  Tech Stack

- Python
- Jupyter Notebook
- pandas / NumPy
- scikit-learn (`KNNImputer`, `MinMaxScaler`)
- SciPy (`scipy.stats`, `scipy.stats.mstats.winsorize`)
- Matplotlib / Seaborn

---

##  Getting Started

1. Extract `Datasets.rar` into the project folder
2. Install dependencies:
   ```bash
   pip install pandas numpy scipy scikit-learn matplotlib seaborn jupyter
   ```
3. Open the notebook:
   ```bash
   jupyter notebook CryptoCurrenciesDatasetCleaning-checkpoint1.ipynb
   ```
4. Run all cells to reproduce the full pipeline, from raw data to the final cleaned, smoothed, and normalized dataset

---

## 🎯 Purpose

This project focuses on data cleaning and preprocessing methodology — comparing multiple techniques at each stage (imputation, outlier handling, smoothing, normalization) and justifying the final choice with quantitative evaluation (RMSE/MAE, skewness, std. deviation) rather than applying a single fixed method blindly.
