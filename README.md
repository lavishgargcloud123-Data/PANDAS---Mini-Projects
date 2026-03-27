# PANDAS
Python Pandas Library - Mini Project for Customer Sales Analysis

DATA Cleaning Methods

1). Data Collection -> Multiple Sources
                    -> Web Scraping
                    -> APIs            ---------> Storage
                    -> Databases                     |
                    -> Surveys                    Raw Data

Now, Raw Data has various kinds of Problems:

-> Spelling Errors
-> Missing Values
-> Outliers
-> Duplicate Values
-> Incorrect Data Types, e.g., Date, Time
-> Inconsistent Formatting

So modelling and analysis get wrong if problems are not rectified...

So, DATA Cleaning is the process of detecting, correcting and handling errors in raw Data to make it suitable for analysis or modelling.


Step 1: Data Collection and Inspection

Type of data in each column.
Check for missing values, etc as in the above list
Check for Data Types
Inconsistencies

Techniques

1-> Summary statistics
2-> Distribution Plot
3-> Information (.info() function -> data types, Null/Not Null, etc)
4-> Frequency Graph -> Categorical Data
5-> Visual Inspection


Step 2: Handling Missing Values

Delete Row or Column directly -> If more than 50% or Data is missing, then better to remove it (row or column)
Imputation Method -> Filling Missing Values
 	Mean, Median, and Mode for Numerical Value Columns
 	-> If outliers, then the median can be used
        -> If no outliers, then the mean can be used
 	-> If Discrete Values, then the Mode can be used
 	-> Unknown
 	-> If a Time-Date dataset is there, Forward Fill / Backward Fill can be used E.g. Stocks Data
 	-> Predictive Imputation -> Machine Learning Models
---
Dropping NAN Values

df.dropna(subset='Column_Name', inplace=True)

2. If there are various Unique Values of single column E.g. Age

df['Age'].unique()

3. Filling the NAN Values

age_median = int(df['Age'].dropna().astype('int64').median())
print(age_median)
df['Age'].fillna(age_median, inplace=True)


## Filling the NAN values by Median Value

NOTE: Mean, Median, and Mode for Numerical Value Columns
 	-> If outliers, then the median can be used
        -> If no outliers, then the mean can be used
 	-> If Discrete Values, then the Mode can be used

4. Filling the Date-Time Column with Forward fill or Backward Fill

df['Last_Purchase_Date'].ffill(inplace=True)

or

df['Last_Purchase_Date'].bfill(inplace=True)

5. Fixing Inconsistent Formatting
 
df['Gender'].unique()

Output -> array(['m ', 'M', 'F', 'FEMALE', 'f ', 'male', 'MALE', 'female'],
          dtype=object)

df['Gender'] = df['Gender'].str.title().str.strip()
df['Gender'].unique()

Output -> array(['M', 'F', 'Female', 'Male'], dtype=object)

## Replace Values:

df['Gender'].replace(['M','F'], ['Male', 'Female'], inplace=True)


5. Handling Duplicates

Remove Duplicate Records/Rows
Check for Primary Key
Remove near duplicate records

6. Handling Outliers

*   Genuine Outliers
*   Errors, e.g., purchase amount in Negative, Age is 250 or Neg.


Using the Box Plot Technique to identify Outliers...
##############################################################

import matplotlib.pyplot as plt
import seaborn as sns

cols = ['Age', 'Purchase_Amount', 'Feedback_Score']

plt.figure(figsize=(15,5))
for i, cols in enumerate(cols, 1):
  plt.subplot(1,3,i)
  sns.boxplot(y=df[cols], color='skyblue', width=0.3)
  plt.title(f'Boxplot of {cols}', fontsize=12)
  plt.ylabel('')
  plt.grid(axis = 'y', linestyle='--', alpha=0.6)

plt.tight_layout()
plt.show()

##############################################################

Using the Z-score method to clear Outliers and remove it...

#####################################################################

from scipy import stats
import numpy as np
z_scores = np.abs(stats.zscore(df[['Age', 'Purchase_Amount']]))
df_clean = df[~(z_scores>3).any(axis=1)]
df_clean.shape

#####################################################################
