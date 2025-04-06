**There is no such thing named perfection, and yet you are still following it, but at what cost**

# 1. A comprehensive review of 'How to do proper EDA' (at least for this project)
(because i have been not doing for too long and i need to review it myself)

## 1.1 Initial Data Inspection & Overview

- What to do:
    + **Dimensions**: Check the number of rows and columns - Understand the scale of the data (observations, features).
    + **Data Types & Non-Null Counts**: Get a concise summary of column types and missing values. Check memory usage
    + **First & Last Rows**: Look at the actual data values
    + **Column Names**: List all column names
    + **Check Unique ID**: Verify the primary key is unique
    + **Columns Type**: Seperate the column into numerical and categorical

## 1.2 Target Variable Analysis
- What to do:
    + **Distribution**: Calculate frequency and proportion of the target classes

## 1.3 Univariate Analysis

### 1.3.1 Numerical Features:

- **Descriptive Statistics**: Get min, max, mean, median, std dev, quartiles.
- **Distributions** (Histograms/KDE)
    + Identify skewness (common in financial data like income, loan amounts), multimodality. Helps inform potential transformations (log, Box-Cox).
    + **Outlier Detection**(Box Plots): Identify extreme values. 
        + Crucial: In credit scoring, outliers aren't always errors; they can represent high-risk/high-value segments. Investigate them rather than blindly removing/capping initially. 
        + Note features with many outliers (e.g., Home Credit's DAYS_EMPLOYED has a specific large value for unemployed).

DAYS_BIRTH, DAYS_EMPLOYED, DAYS_REGISTRATION, DAYS_ID_PUBLISH

REG_REGION_NOT_LIVE_REGION, REG_REION_NOT_WORK_REGION, 
REG_CITY_NOT_LIVE_CITY, REG_CITY_NOT_WORK_CITY, 
LIVE_REGION_NOT_WORK_REGION, LIVE_CITY_NOT_WORK_CITY -> Change to categorical

REGION_RATING_CLIENT, REGION_RATING_CLIENT_W_CITY -> Change to categorical

AMT_REQ_CREDIT_BUREAU_HOUR, AMT_REQ_CREDIT_BUREAU_DAY, AMT_REQ_CREDIT_BUREAU_WEEK, AMT_REQ_CREDIT_BUREAU_QRT, AMT_REQ_CREDIT_BUREAU_YEAR -> Change to categorical

### 1.3.2 Categorical Features:
- **Value Counts & Cardinality**: Check unique values and their frequencies for each categorical column.
    + Understand the different categories and their prevalence. 
    + Identify high cardinality features (many unique values, e.g., occupation) which might need special encoding. 
    + Identify rare categories which might need grouping. 
    + Check for potential data entry errors (e.g., 'Male', 'male', 'M'). 
    + Note missing values represented as categories (e.g., 'XNA').

## 1.4 Bivariate Analysis

- **Numerical vs. Target**: How do numerical features relate to the likelihood of default?
    + Identify features that show different distributions between defaulters and non-defaulters. This indicates predictive power. Look for expected relationships (e.g., lower income associated with higher default).
- **Categorical vs. Target**: How do different categories relate to the likelihood of default?
    + Identify potential multicollinearity, which can affect the stability and interpretation of some models (like Logistic Regression). Understand relationships between predictors.
- **Categorical vs. Categorical**: Explore relationships between key categorical features.
    + Understand how different categorical segments overlap. Might reveal interesting interactions or redundant information
- **Numerical vs. Numerical**: Explore relationships between key numerical features.

## 1.5 Data Quality

### 1.5.1 Missing Values Analysis

- **Calculate Percentage and Visualize Missingness**:
    + Understand the extent and pattern of missing data. 
    + Are values missing completely at random (MCAR), at random (MAR), or not at random (MNAR)? 
    + This informs imputation strategy. High percentages (>40-50%) might warrant feature removal. 
    + Correlations in missingness can suggest underlying reasons.
- **Duplicate Rows**: Check for duplicate rows in the dataset. 
    + If duplicates exist, decide whether to drop them or keep them based on the context of the data.
- **Outlier Investigation**: Revisit features identified with potential outliers in Phase 3.
    + *Action*: Analyze specific extreme values.
    + Decide on a preliminary handling strategy (investigate further, cap, transform, leave as is for tree models). + Document your reasoning.
    + Example: df_app_train['DAYS_EMPLOYED'].value_counts().head() might reveal the special value.
- **Consistency Checks**: Apply domain knowledge to check for logical inconsistencies
    + Examples: DAYS_BIRTH should always be negative and increase over time (less negative). DAYS_EMPLOYED should generally be less than DAYS_BIRTH in magnitude. Check the special value. Amounts (AMT_...) should generally be non-negative.
    + *Action*: Investigate any inconsistencies found. They might indicate data errors needing correction or specific definitions (like the DAYS_EMPLOYED flag).

## 1.6 Summary and Documentation:

- **Summarize Key Findings**: Write down the most important observations in markdown cells within your notebook
    + Target variable imbalance ratio.
    + Key numerical features and their distributions/outliers/relationship with target.
    + Key categorical features and their cardinality/relationship with target.
    + Features with significant missing data and potential handling strategies.
    + Identified outliers and potential handling strategies.
    + Highly correlated numerical features.
    + Any data quality issues or inconsistencies noted.

- **Initial Hypotheses**: Formulate initial ideas about which features seem most predictive.

- **Preprocessing Plan**: Based on the EDA, outline the necessary preprocessing steps:
    + Handling missing values (imputation methods per feature type or group).
    + Outlier treatment strategy (if any).
    + Categorical encoding strategy (One-Hot, WOE, Target Encoding).
    + Numerical transformations (scaling, log transform for skewness).
    + Feature engineering ideas (creating ratios, interaction terms, polynomial features - more relevant after merging bureau data).

