
# 📉 Churn Predictor
This project performs customer churn prediction using data preprocessing, feature engineering, exploratory analysis, and correlation analysis on telecom customer data. The aim is to understand key drivers behind customer churn and prepare the dataset for further modeling.

## 🧾 Project Structure
- Dataset: Includes customer demographics, subscription details, and churn information.

- Notebook: Contains steps for cleaning, encoding, binning, and correlation analysis.

- This README: Provides an overview of methodology and file usage.
  
---

## 📊 Key Steps Explained
### 1. Data Cleaning
Missing values handled:

- **For categorical columns**: filled with mode.

- **For numerical columns**: filled with mean.

```
python

df[column].fillna(df[column].mode()[0], inplace=True) 
```

### 2. Encoding Categorical Variables
Label Encoding was initially applied but .corr() only works with numeric types.

- So, One-Hot Encoding (pd.get_dummies()) was used instead to convert object types to binary columns.

```
python

df = pd.get_dummies(df, drop_first=True)
```
### 3. Tenure Binning
Customers grouped into tenure ranges using pd.cut().

 - Used include_lowest=True to include the lowest tenure value in the first bin.

```
python

bins = [0, 12, 24, 36, 48, 60, 72]
labels = ['0-12','12-24','24-36','36-48','48-60','60-72']
df['tenure_bin'] = pd.cut(df['tenure_months'], bins=bins, labels=labels, include_lowest=True)
```

Unequal intervals work too — pd.cut() does not require equal-width bins.

### 4. Grouping by Tenure Bins
Count of customers in each tenure group was calculated:

```
python

df.groupby('tenure_bin').size()
```

Reordered to put tenure_bin as the first column:

```
python

df.drop('tenure_months', axis=1, inplace=True)
df = df[['tenure_bin'] + [col for col in df.columns if col != 'tenure_bin']]
```

### 5. Correlation Analysis
.corr() applied only after converting all non-numeric columns using One-Hot Encoding.

```
python

corr_matrix = df.corr()
corr_with_churn = corr_matrix['is_churned'].sort_values(ascending=False)
```

### 🧠 Learnings
- Avoid LabelEncoder for direct correlation work unless you're dealing with ordinal data.

- Use pd.get_dummies() + drop_first=True to reduce multicollinearity.

- Bin variables meaningfully to analyze customer groups.

### 📂 Files
- churnPredictorIndex.ipynb: Main notebook with all preprocessing steps

- README.md: This explanation

- data.csv (not included): Assumed data source
