# Belfast House Price Analysis & Prediction

An end-to-end data science project that explores 20 years of Belfast residential property sales (2005–2024) and builds a Linear Regression model to predict the annual **median sale price**.

The project was completed as coursework for the **Data Science Engineering** module (Level 6, Year 1). It works through the full data science workflow: loading data, exploratory analysis, cleaning, feature engineering, modelling and evaluation.

---

## Purpose of the Exercise

The aim is to show the core stages of a data science pipeline on a real-world dataset:

1. **Load and inspect** a raw CSV dataset
2. **Explore** it with summary statistics, correlations and visualisations
3. **Clean and prepare** it: missing values, categorical encoding, outlier removal and scaling
4. **Model** the median house price with Linear Regression
5. **Evaluate** the model with standard regression metrics and explain the results

The notebook focuses on a clear, reproducible method rather than on getting the most accurate model possible.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| **Python 3** | Core language |
| **Jupyter Notebook** | Interactive analysis and documentation |
| **pandas** | Loading and manipulating the data |
| **NumPy** | Numerical operations |
| **Matplotlib** | Plotting (trend lines, histograms, boxplots) |
| **Seaborn** | Statistical visualisation (correlation heatmap) |
| **scikit-learn** | Preprocessing (`StandardScaler`, `LabelEncoder`), modelling (`LinearRegression`) and metrics (RMSE, MAE, R²) |
| **statsmodels** / **joblib** | Listed in `requirements.txt` for statistical extensions and saving models |

---

## Dataset

**File:** `belfast_all-sales.csv`

Annual summary statistics for verified residential property sales in the Belfast district, one row per year from **2005 to 2024** (20 rows, 10 columns, no missing values).

| Column | Description |
|--------|-------------|
| `District` | Local government district (Belfast) |
| `SaleYr` | Year of sale |
| `Type` | Sale type (`All Sales`) |
| `LGD_VerifiedSales` | Number of verified sales in the year |
| `Reporting_MinPrice` | Minimum sale price (£) |
| `Reporting_LQPrice` | Lower quartile price (£) |
| `Reporting_MedianPrice` | Median price (£), **the target variable** |
| `Reporting_MeanPrice` | Mean price (£) |
| `Reporting_UQPrice` | Upper quartile price (£) |
| `Reporting_MaxPrice` | Maximum sale price (£) |

---

## Project Structure

```
belfast-house-price-analysis/
├── belfast-house-price-analysis.ipynb   # Main analysis notebook
├── belfast_all-sales.csv                # Dataset
├── requirements.txt                     # Python dependencies
└── ReadMe.md
```

---

## Notebook Walkthrough

### 1. Importing Libraries & Loading Data
Imports the libraries, loads the CSV and prints its shape, data types and first few rows.

### 2. Exploratory Data Analysis (EDA)
- Summary statistics and DataFrame info
- Missing value check
- Correlation matrix and heatmap
- Histograms of the key price and sales-volume features
- Boxplots to spot outliers
- **Median price trend from 2005 to 2024**, showing the peak around 2007, the fall after the 2008 financial crisis and the later recovery

### 3. Data Cleaning & Preparation
- Works on a copy so the raw data stays untouched
- Fills any missing numeric values with the column mean
- Label-encodes the categorical columns (`District`, `Type`)
- Removes outliers with the **IQR method** on the price columns (removes 1 extreme `MaxPrice` year)
- Uses `Reporting_MedianPrice` as the target
- **Time-based train/test split**: trains on earlier years and tests on the most recent year (2024), which avoids look-ahead bias
- Standardises the features with `StandardScaler`, fitted on the training data only

### 4. Model Development
- **Linear Regression** baseline
- A **log-transformed target** variant for comparison
- A comparison table that picks the better model

### 5. Model Evaluation & Visualisation
- Actual vs predicted scatter plot
- An explanation of how to read **RMSE**, **MAE** and **R²**

---

## Results

| Model | Test RMSE | Test MAE |
|-------|-----------|----------|
| Linear Regression | £632 | £632 |
| Linear Regression (log-target) | £4,628 | £4,628 |

The standard Linear Regression model gives the lower error on the 2024 test year. Its training R² is 0.997.

---

## Limitations & Possible Improvements

These limitations are listed so readers can interpret the results correctly:

- **Small dataset.** With only 20 annual rows, the model has very little data to learn from. With 19 rows left after outlier removal, the time-based split leaves a **single test sample**. As a result, **test R² cannot be calculated (shows `NaN`)**, and RMSE and MAE are both the error on that one prediction.
- **Feature leakage.** Features such as the mean, lower quartile and upper quartile prices come from the same yearly sales as the median target. They are therefore very strongly correlated with it, which explains the near-perfect training fit. A forecasting model should use only information known *before* the target year, such as lagged prices, interest rates, inflation or housing supply.
- **Constant columns.** `District` and `Type` have a single value each, so encoding them adds no information.

**Possible next steps:**
- Use more detailed data (e.g. individual transactions, property type or postcode-level data)
- Engineer lagged or rolling features for a proper time-series forecast
- Use time-series cross-validation instead of a single test year
- Compare against other models (Ridge/Lasso, Random Forest, Gradient Boosting, ARIMA)
- Save the trained model with `joblib` for reuse

---

## Getting Started

### Prerequisites
- Python 3.9+
- Jupyter Notebook or JupyterLab (or VS Code with the Jupyter extension)

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/belfast-house-price-analysis.git
cd belfast-house-price-analysis

# (Optional) create a virtual environment
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
pip install notebook
```

### Run the Notebook

```bash
jupyter notebook belfast-house-price-analysis.ipynb
```

Run the cells from top to bottom. The CSV must stay in the same folder as the notebook.

---

## Author

**Jamie Ferguson**
Data Science Engineering coursework, HLA Top Up (Level 6)
