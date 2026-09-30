# PCA Formative 2 — Team 28

## 1. Project Overview

This project implements Principal Component Analysis (PCA) on an Africanized subset of the World Development Indicators (WDI) dataset. The analysis focuses on **population density (people per sq. km of land area)** across African countries over multiple years.

The main purpose of the project is to demonstrate the PCA workflow:

1. Load and filter the dataset.
2. Clean and prepare the numerical data.
3. Handle missing values.
4. Standardize the features.
5. Calculate the covariance matrix.
6. Perform eigendecomposition.
7. Sort eigenvalues and eigenvectors.
8. Select principal components.
9. Project the standardized data into the reduced space.
10. Visualize the data before and after PCA.
11. Interpret the dimensionality reduction and information loss.

---

## 2. Dataset

### Source

The dataset is from the **World Development Indicators (WDI)** and is provided in the project as:

`API_16_DS2_en_csv_v2_264259.csv`

The dataset reports a **Last Updated Date of 4/8/2026**.

### Indicator Used

- **Indicator:** Population density (people per sq. km of land area)
- **Indicator Code:** `EN.POP.DNST`
- **Unit:** People per square kilometre of land area

### Original Dataset Structure

The supplied WDI file contains:

- Country Name
- Country Code
- Indicator Name
- Indicator Code
- Yearly observations from **1960 to 2025**

### Dataset Used in PCA

The notebook filters the original dataset to:

- **54 African countries**
- Population Density indicator (`EN.POP.DNST`)
- Years **1961–2023**

The years 1960, 2024, and 2025 were removed because they contained no usable population-density values across the selected African-country subset.

After filtering and cleaning, the PCA input contains:

- **54 observations (countries)**
- **63 numerical features (years)**

Therefore, the standardized dataset has the shape:

`(54, 63)`

---

## 3. Why PCA Is Used

The dataset contains 63 yearly features for each country. Many neighbouring years contain highly related population-density information, creating substantial redundancy.

PCA is used to transform these correlated yearly features into a smaller set of principal components while retaining most of the variation in the data.

In this project, PCA allows the 63-dimensional dataset to be represented using two principal components for visualization and interpretation.

---

## 4. Data Preparation

The project follows these preprocessing steps:

### 4.1 Load the Data

The CSV file is loaded using NumPy. The first four rows of the WDI file are skipped because they contain metadata before the main table.

### 4.2 Filter African Countries

The analysis uses a list of 54 African ISO3 country codes.

### 4.3 Filter the Indicator

Only the population-density indicator is retained:

`EN.POP.DNST`

### 4.4 Convert Values to Numeric

The yearly values are converted to floating-point numbers. Values that cannot be converted are represented as missing values (`NaN`).

### 4.5 Remove Completely Empty Columns

Any year containing no usable values across the selected countries is removed.

This results in the 63 usable yearly features from 1961 through 2023.

### 4.6 Handle Missing Values

Remaining missing values are replaced with the mean of their respective year/feature column.

### 4.7 Standardization

Each feature is standardized using:

```text
Z = (X - mean) / standard deviation
```

This gives the features a mean approximately equal to 0 and a standard deviation approximately equal to 1.

---

## 5. PCA Methodology

### Step 1 — Standardize the Data

The standardized matrix has the shape:

```text
54 × 63
```

Each row represents an African country, while each column represents a year.

### Step 2 — Calculate the Covariance Matrix

The covariance matrix is calculated from the transpose of the standardized data:

```python
cov_matrix = np.cov(standardized_data.T)
```

Because there are 63 features, the covariance matrix has dimensions:

```text
63 × 63
```

The covariance matrix captures relationships between the yearly population-density features.

### Step 3 — Eigendecomposition

The covariance matrix is decomposed into eigenvalues and eigenvectors:

```python
eigenvalues, eigenvectors = np.linalg.eig(cov_matrix)
```

- Eigenvalues indicate how much variance is associated with each principal component.
- Eigenvectors define the directions of the principal components.

### Step 4 — Sort the Principal Components

The eigenvalues are sorted in descending order, and the corresponding eigenvectors are rearranged in the same order.

The principal component associated with the largest eigenvalue is placed first.

### Step 5 — Select Principal Components

The project selects the first **two principal components**.

This allows the 63-dimensional dataset to be represented in two dimensions.

### Step 6 — Project the Data

The standardized data is projected onto the selected eigenvectors using a dot product:

```python
reduced_data = np.dot(standardized_data, selected_components)
```

The resulting reduced dataset has the shape:

```text
54 × 2
```

---

## 6. Results

The first two principal components account for approximately:

**99.7% of the total variance**

More precisely, the notebook's eigenvalues give approximately:

- **PC1:** 97.2% of total variance
- **PC2:** 2.5% of total variance
- **PC1 + PC2:** approximately 99.7%

This means that two principal components preserve almost all of the variation captured by the original 63 yearly features.

The remaining approximately **0.3%** of variance is represented by PC3 through PC63.

---

## 7. Visualization

The project compares the data before and after PCA.

### Before PCA

The original data contains 63 yearly dimensions. The notebook visualizes the strong similarity between neighbouring years, with years such as 1961 and 1962 showing highly redundant population-density patterns.

### After PCA

The data is represented using:

- Principal Component 1 on the x-axis
- Principal Component 2 on the y-axis

This produces a two-dimensional representation of the 54 African countries based on their overall historical population-density patterns.

The notebook also labels observations that fall beyond the specified PC1/PC2 thresholds as potential outliers.

---

## 8. Interpretation

The first principal component primarily represents the dominant variation in population density across the historical years.

The second principal component captures an additional independent pattern in the yearly population-density trajectories.

The PCA transformation therefore changes the representation from 63 individual yearly features into two combined dimensions that summarize most of the variation in the dataset.

---

## 9. Information Lost Through Dimensionality Reduction

Reducing 63 yearly features to two principal components removes approximately 0.3% of the total variance.

The information lost includes smaller year-specific variations and short-term demographic changes that are not strongly represented by the first two principal components.

For example, temporary population-density increases or decreases associated with localized demographic events may be less visible after the reduction.

The tradeoff is between retaining every individual yearly detail and obtaining a much simpler two-dimensional representation that is easier to visualize and interpret.

---

## 10. Technologies Used

- **Python 3.8.8**
- **NumPy**
- **Google Colab / Jupyter Notebook**
- **Matplotlib** for visualization

The assignment instructions specifically require that no libraries other than NumPy be used for the PCA implementation.

---

## 11. Project Files

```text
.
├── PCA_Formative_2[Team_28].ipynb
├── API_16_DS2_en_csv_v2_264259.csv
└── README.md
```

### File Descriptions

| File | Description |
|---|---|
| `PCA_Formative_2[Team_28].ipynb` | Jupyter/Google Colab notebook containing the PCA implementation, outputs, explanations, and visualizations. |
| `API_16_DS2_en_csv_v2_264259.csv` | World Development Indicators dataset used as the source data. |
| `README.md` | Documentation describing the project, dataset, methodology, results, and usage. |

---

## 12. How to Run the Project

### Option 1 — Google Colab

1. Open the notebook in Google Colab.
2. Upload `API_16_DS2_en_csv_v2_264259.csv` to the working directory, or clone the project repository if it is hosted on GitHub.
3. Run the notebook cells in order.
4. Ensure that the dataset filename matches:

```text
API_16_DS2_en_csv_v2_264259.csv
```

### Option 2 — Jupyter Notebook

Install Python and NumPy, place the notebook and CSV file in the same directory, and run the notebook from Jupyter.

---

## 13. Reproducibility

The PCA workflow can be reproduced from the supplied notebook and dataset.

The main processing sequence is:

```text
WDI Dataset
     ↓
Select African Countries
     ↓
Select Population Density
     ↓
Select Usable Years
     ↓
Handle Missing Values
     ↓
Standardize Data
     ↓
Covariance Matrix
     ↓
Eigendecomposition
     ↓
Sort Eigenvalues/Eigenvectors
     ↓
Select PC1 and PC2
     ↓
Project Data
     ↓
Visualize and Interpret
```

---

## 14. Limitations

- The analysis focuses on only one WDI indicator: population density.
- PCA components are mathematical combinations of the original yearly variables and do not directly represent individual years.
- Reducing 63 dimensions to two removes a small amount of information.
- Mean imputation replaces missing observations with column means and may reduce some of the natural variation in the data.
- The analysis is descriptive and does not establish causal relationships between population density and other socioeconomic factors.

---

## 15. Team

**Team 28**

Team members:

- Brian Kiguru Mahui
- Garang Wanambisi Buke

---

## 16. Assignment Requirements Followed

The notebook follows the provided PCA assignment requirements, including:

- Displaying outputs for code cells.
- Separating code into multiple cells.
- Implementing PCA using NumPy.
- Standardizing the dataset.
- Computing the covariance matrix.
- Performing eigendecomposition.
- Sorting principal components.
- Projecting the data onto selected components.
- Producing before-and-after PCA visualizations.
- Explaining the choice of principal components and the information lost through dimensionality reduction.

---

## 17. Repository

GitHub repository:

**https://github.com/garangbse/PCA_Formative-Team_28**
